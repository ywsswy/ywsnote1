class MockClass1 : public ::testing::Test {
 protected:
  static void SetUpTestCase() {
    // run before first test case
    std::cout << "SetUpTestCase:" << count++ << std::endl;
  }

  static void TearDownTestCase() {
    // run after last test case
    std::cout << "TearDownTestCase:" << count++ << std::endl;
  }
  virtual void SetUp() {
    // run before every test case
    std::cout << "SetUp:" << count++ << std::endl;
  }

  virtual void TearDown() {
    // run after every test case
    std::cout << "TearDown:" << count++ << std::endl;
  }
 private:
  static int count;
};