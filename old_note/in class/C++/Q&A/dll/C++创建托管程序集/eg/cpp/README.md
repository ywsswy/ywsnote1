// 这是主 DLL 文件。
#include "stdafx.h"
#include "0422car.h"
#include <iostream>
using namespace std;
namespace My0422car{
	int  CarDll::CarConnect() {
		WSADATA wsaData;
		SOCKADDR_IN addrServer;//服务端地址
		WSAStartup(MAKEWORD(2, 2), &wsaData);
		//新建客户端socket
		sockClient = socket(AF_INET, SOCK_STREAM, 0);
		//定义要连接的服务端地址
		addrServer.sin_addr.S_un.S_addr = inet_addr("192.168.8.1");
		addrServer.sin_family = AF_INET;
		addrServer.sin_port = htons(2001);
		return connect(sockClient, (SOCKADDR*)&addrServer, sizeof(SOCKADDR));
	}
	int CarDll::move(int i) {
		switch (i)
		{
		case 0:
			sendmsg("go");
			break;
		case 1:
			sendmsg("go1");
			break;
		default:
			break;
		}
		char r[2];
		r[1] = '\0';
		for (;;) {
			receiveMsg(r);
			if (r[0] == 'r') {
				return 0;
			}
			else if (r[0] == 'e') {
				return 1;
			}
		}
		return -1;
	}
	int CarDll::servo_move(int parameter1, int parameter2) {
		char c1[10], c2[10];
		char a[] = ",";
		_itoa_s(parameter1, c1, 10);
		_itoa_s(parameter2, c2, 10);
		strcat_s(c1, a);
		strcat_s(c1, c2);
		return sendmsg(c1);
	}
	int CarDll::servo_reverse() {
		return sendmsg("94,80");
	}
	int CarDll::receiveMsg(char* recvmsg) {
		return recv(sockClient, recvmsg, 1, 0);
	}
	int CarDll::sendmsg(char sendmsg[]) {
		return send(sockClient, sendmsg, strlen(sendmsg) + 1, 0);
	}
	bool CarDll::CapturePic(int i) {
		char addr[] = "D://tmp";
		LPCTSTR szUrl = _T("http://192.168.8.1:8083/?action=snapshot");
		TCHAR tAddr[40];
		TCHAR szLocalFile[40];
		szLocalFile[0] = '\0';
		HANDLE hFile = INVALID_HANDLE_VALUE;
		HINTERNET hInet = NULL;
		HINTERNET hUrl = NULL;
		DWORD dwBuf = 1024 * 1024, dwRead = 0; //1M
		std::auto_ptr<char> szBuf(new char[dwBuf]);
		memset(szBuf.get(), 0, dwBuf);
		std::string strTmp;
		bool bRet = false;
		CharToTchar(addr, tAddr, 7);
		lstrcat(szLocalFile, tAddr);
		getFileAddr(szLocalFile, i);
		if (PathFileExists(szLocalFile)) {
			char* fileName = ConvertLPWSTRToLPSTR(szLocalFile);
			remove(fileName);
		}
		if (
			!PathIsDirectory(szLocalFile) &&
			(GetFileAttributes(szLocalFile) != INVALID_FILE_ATTRIBUTES))
		{
			return false;
		}
		try
		{
			hFile = CreateFile(szLocalFile, GENERIC_READ | GENERIC_WRITE, 0, NULL, CREATE_ALWAYS, FILE_ATTRIBUTE_NORMAL, NULL);
			if (hFile == INVALID_HANDLE_VALUE)
				throw "error";
			hInet = InternetOpen(NULL, INTERNET_OPEN_TYPE_PRECONFIG, NULL, NULL, 0);
			if (hInet == NULL) {
				cout << "hinet e" << endl;
				throw "error";
			}
			hUrl = InternetOpenUrl(hInet, szUrl, NULL, 0,
				INTERNET_FLAG_TRANSFER_BINARY | INTERNET_FLAG_NO_CACHE_WRITE | INTERNET_FLAG_RELOAD, 0);
			if (hUrl == NULL) {
				cout << "hurl e" << endl;
				throw "error";
			}
			for (;;)
			{
				if (!InternetReadFile(hUrl, szBuf.get(), dwBuf, &dwRead))
				{
					bRet = false;
					break;
				}
				if (dwRead == 0)
				{
					bRet = true;
					break;
				}
				//strTmp += std::string(szBuf,dwRead);
				WriteFile(hFile, szBuf.get(), dwRead, &dwRead, NULL);
			}
			throw "ok";
		}
		catch (...)
		{
			if (hFile != INVALID_HANDLE_VALUE)
				CloseHandle(hFile);
			if (hUrl != NULL)
				InternetCloseHandle(hUrl);
			if (hInet != NULL)
				InternetCloseHandle(hInet);
		}
		return bRet;
	}
	void CarDll::getFileAddr(TCHAR* addr, int i) {
		TCHAR* saveFile = new TCHAR[20];
		saveFile[0] = _T('\0');
		TCHAR* s1 = _T("/pic");
		TCHAR* s2 = _T(".jpg");
		TCHAR s3[2];
		s3[1] = _T('\0');
		const char no = '0' + i;
		CarDll::CharToTchar(&no, s3, 1);
		lstrcat(saveFile, s1);
		lstrcat(saveFile, s3);
		lstrcat(saveFile, s2);
		lstrcat(addr, saveFile);
	}
	void CarDll::CharToTchar(const char * _char, TCHAR * tchar, int i)
	{
		while (i-- != 0) {
			MultiByteToWideChar(CP_ACP, 0, _char++, -1, tchar++, 1);
		}
		*tchar = '\0';
	}
	char* CarDll::ConvertLPWSTRToLPSTR(LPWSTR lpwszStrIn)
	{
		LPSTR pszOut = NULL;
		if (lpwszStrIn != NULL)
		{
			int nInputStrLen = wcslen(lpwszStrIn);
			// Double NULL Termination  
			int nOutputStrLen = WideCharToMultiByte(CP_ACP, 0, lpwszStrIn, nInputStrLen, NULL, 0, 0, 0) + 2;
			pszOut = new char[nOutputStrLen];
			if (pszOut)
			{
				memset(pszOut, 0x00, nOutputStrLen);
				WideCharToMultiByte(CP_ACP, 0, lpwszStrIn, nInputStrLen, pszOut, nOutputStrLen, 0, 0);
			}
		}
		return pszOut;
	}
	CarDll::CarDll()
	{
	}
	CarDll::~CarDll()
	{
	}
}
