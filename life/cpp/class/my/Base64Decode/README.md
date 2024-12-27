std::string Base64Encode(const std::string& source) {
  int byte_src = source.length();
  int i = 0;
  for (i; i < byte_src; i++)
    if (source[i] < 0 || source[i] > 127) throw "can not encode Non-ASCII characters";

  const char* enkey = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/";
  std::string psz_encode(byte_src * 4 / 3 + 4, '\0');
  int nLoop = byte_src % 3 == 0 ? byte_src : byte_src - 3;
  int n = 0;
  for (i = 0; i < nLoop; i += 3) {
    psz_encode[n] = enkey[source[i] >> 2];
    psz_encode[n + 1] = enkey[((source[i] & 3) << 4) | ((source[i + 1] & 0xF0) >> 4)];
    psz_encode[n + 2] =
        enkey[((source[i + 1] & 0x0f) << 2) | ((source[i + 2] & 0xc0) >> 6)];
    psz_encode[n + 3] = enkey[source[i + 2] & 0x3F];
    n += 4;
  }

  switch (byte_src % 3) {
    case 0:
      psz_encode[n] = '\0';
      break;

    case 1:
      psz_encode[n] = enkey[source[i] >> 2];
      psz_encode[n + 1] = enkey[((source[i] & 3) << 4) | ((0 & 0xf0) >> 4)];
      psz_encode[n + 2] = '=';
      psz_encode[n + 3] = '=';
      psz_encode[n + 4] = '\0';
      break;

    case 2:
      psz_encode[n] = enkey[source[i] >> 2];
      psz_encode[n + 1] = enkey[((source[i] & 3) << 4) | ((source[i + 1] & 0xf0) >> 4)];
      psz_encode[n + 2] = enkey[((source[i + 1] & 0xf) << 2) | ((0 & 0xc0) >> 6)];
      psz_encode[n + 3] = '=';
      psz_encode[n + 4] = '\0';
      break;
  }
  return psz_encode.c_str();
}

std::string Base64Decode(const std::string& origin) {
  std::string result;
  if (origin.size() % 4 != 0) {
    return result;
  }
  result.resize(origin.size() / 4 * 3);
  static constexpr int kIndexes[] = {
      -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1,
      -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, 62,
      -1, -1, -1, 63, 52, 53, 54, 55, 56, 57, 58, 59, 60, 61, -1, -1, -1, -1, -1, -1, -1, 0,
      1,  2,  3,  4,  5,  6,  7,  8,  9,  10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22,
      23, 24, 25, -1, -1, -1, -1, -1, -1, 26, 27, 28, 29, 30, 31, 32, 33, 34, 35, 36, 37, 38,
      39, 40, 41, 42, 43, 44, 45, 46, 47, 48, 49, 50, 51, -1, -1, -1, -1, -1, -1, -1, -1, -1,
      -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1,
      -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1,
      -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1,
      -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1,
      -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1,
      -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1};
  auto p = result.begin();
  auto first = origin.begin();
  for (; first != origin.end();) {
    uint32_t n = 0;
    for (int i = 1; i <= 4; ++i, ++first) {
      auto idx = kIndexes[static_cast<size_t>(*first)];
      if (idx == -1) {
        if (i <= 2) {
          result.erase(p, result.end());
          return result;
        }
        if (i == 3) {
          if (*first == '=' && *(first + 1) == '=' && first + 2 == origin.end()) {
            *p++ = n >> 16;
            result.erase(p, result.end());
            return result;
          }
          result.erase(p, result.end());
          return result;
        }
        if (*first == '=' && first + 1 == origin.end()) {
          *p++ = n >> 16;
          *p++ = n >> 8 & 0xffu;
          result.erase(p, result.end());
          return result;
        }
        result.erase(p, result.end());
        return result;
      }
      n += idx << (24 - i * 6);
    }
    *p++ = n >> 16;
    *p++ = n >> 8 & 0xffu;
    *p++ = n & 0xffu;
  }
  result.erase(p, result.end());
  return result;
}