bool ReadTextFile(const std::string &filename, std::vector<std::string> &data)
{
    std::ifstream ifs(filename.c_str());
    if (!ifs.is_open())
    {
        std::cerr << "file open failed, " << filename << std::endl;
        return false;
    }
    else
    {
        std::string line_data;
        while(getline(ifs, line_data))
        {
            data.push_back(line_data);
        }
        ifs.close();
        return true;
    }
}