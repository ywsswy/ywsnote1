// 0422car.h
#pragma once
#include <windows.h>
#include <cstdio>
#include <iostream>
#include <string>
#include <vector>
#include <algorithm>
#include <wininet.h>
#include <shellApi.h>
#include <ShlObj.h>
#include <tchar.h>
#include <stdlib.h>
#include "shlwapi.h"
#pragma comment(lib,"shlwapi.lib")
#pragma comment(lib,"wininet")
#pragma comment(lib,"ws2_32.lib")
#ifdef CARDLL_EXPORTS
#define CARDLL_API __declspec(dllexport)
#else
#define CARDLL_API __declspec(dllimport)
#endif
using namespace System;
namespace My0422car {
	public ref class CarDll
	{
	public:
		CarDll();
		int  CarConnect();
		int move(int i);
		int servo_move(int parameter1, int parameter2);
		int servo_reverse();
		int receiveMsg(char* recvmsg);
		int sendmsg(char sendmsg[]);
		bool CapturePic(int i);
		//bool savePic(TCHAR* addr, int i, BOOL bFailIfExists);
		~CarDll();
	private:
		SOCKET sockClient;
		void getFileAddr(TCHAR* addr, int i);
		void CharToTchar(const char * _char, TCHAR * tchar, int i);
		char* ConvertLPWSTRToLPSTR(LPWSTR lpwszStrIn);
	};
}

