 <widget id="A6931917364345"  version="0.0.1">
//必须与云端创建的一致//apploader会在sdcard/UZmap/wgt文件夹下创建AA6931917364345放置同步文件
    <name>我想对你说</name>
    <description>
        Example For APICloud.
    </description>
    <author email="developer@apicloud.com" href="http://www.apicloud.com">
        Developer
    </author>
    <content src="./html/hello.html" />
//特别注意，‘.’表示config.xml所在目录，不可省略不写哦(￣▽￣)"
    <access origin="*" />
    <preference name="pageBounce" value="false"/>
	<preference name="appBackground" value="rgba(0,0,0,0.0)"/>
	<preference name="windowBackground" value="#fff"/>
//主色调为白色
	<preference name="frameBackgroundColor" value="rgba(0,0,0,0.0)"/>
	<preference name="hScrollBarEnabled" value="true"/>
	<preference name="vScrollBarEnabled" value="true"/>
	<preference name="autoLaunch" value="true"/>
	<preference name="fullScreen" value="false"/>
	<preference name="autoUpdate" value="true" />
	<preference name="smartUpdate" value="false" />
	<preference name="debug" value="true"/>
	<preference name="statusBarAppearance" value="true"/>
	<permission name="readPhoneState" />
	<permission name="camera" />
	<permission name="record" />
	<permission name="location" />
	<permission name="fileSystem" />
	<permission name="internet" />
	<permission name="bootCompleted" />
	<permission name="hardware" />
</widget>

