#include <iostream>
#include <unistd.h>

class Args
{
public:
    Args() {}
    bool help();
    bool ParseArgs(int argc, char* argv[]);
    std::string input;
    std::string type;
};

bool Args::help()
{
    std::cout << "usage: ./main [-h] [-t type] [-i input]" << std::endl;
    return false;
}
bool Args::ParseArgs(int argc, char* argv[])
{
    int ch; 
    while ((ch = getopt(argc, argv, "t:i:h")) != -1) 
    {   
        switch (ch)
        {   
        case 't':
            type= optarg;
            break;
        case 'i':
            input = optarg;
            break;
        case 'h':
        default:
            return help();
        }   
    }   
    if (type != "en" && type != "de")
    {   
        return help();
    }   
    return true;
}

int main(int argc, char* argv[])
{
    Args info;
    if (!info.ParseArgs(argc, argv))
    {   
        exit(-1);
    }   
    return 0;
}