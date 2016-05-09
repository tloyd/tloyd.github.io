---
layout: post
title:  "Android API Guide:Storage Options 中文翻译"
date:   2016-05-10 12:14:46
categories: 文档翻译
tags:  AndroidAPI Storage
author: 何帆
---

* content
{:toc}  

Android提供了几种方式来存储你的应用数据。你可以根据你的需求来选择解决方案，比如数据是否为应用所私有或者是否能被其他应用（或用户）读取或者数据所需要的空间大小。




## Storage Options 

> Android provides several options for you to save persistent application data. The solution you choose depends on your specific needs, such as whether the data should be private to your application or accessible to other applications (and the user) and how much space your data requires.

android提供了几种方式来存储你的应用数据。你可以根据你的需求来选择你的解决方案，比如数据是否为应用所私有或者是否能被其他应用（或用户）读取和数据所需要的空间大小。


> Your data storage options are the following:
> 
> [Shared Preferences ](https://developer.android.com/intl/zh-cn/guide/topics/data/data-storage.html#pref)  
> 　　Store private primitive data in key-value pairs.  
> [Internal Storage](https://developer.android.com/intl/zh-cn/guide/topics/data/data-storage.html#filesInternal)  
> 　　Store private data on the device memory.  
> [External Storage](https://developer.android.com/intl/zh-cn/guide/topics/data/data-storage.html#filesExternal)  
> 　　Store public data on the shared external storage.  
> [SQLite Databases](https://developer.android.com/intl/zh-cn/guide/topics/data/data-storage.html#db)  
> 　　Store structured data in a private database.  
> [Network Connection](https://developer.android.com/intl/zh-cn/guide/topics/data/data-storage.html#netw)  
> 　　Store data on the web with your own network server.  

存储方式有如下几种：

共享参数  
　　用键值对的方式来储存原始的私有数据  
内部存储空间  
　　在设备内部存储空间里存储私有数据  
外部存储空间  
　　在设备外部共享空间里存储公有数据  
SQLite数据库  
　　在私有数据库里保存结构化的数据  
网络连接  
　　在网络服务器上存储数据  

> Android provides a way for you to expose even your private data to other applications — with a [content provider](https://developer.android.com/guide/topics/providers/content-providers.html). A content provider is an optional component that exposes read/write access to your application data, subject to whatever restrictions you want to impose. For more information about using content providers, see the [Content Providers](https://developer.android.com/guide/topics/providers/content-providers.html) documentation.

Android提供了一个为其他应用暴露私有数据的方法：[content provider](https://developer.android.com/guide/topics/providers/content-providers.html)。content provider是一个可选的组件。它在你施加的限制条件下，用来向外界暴露应用程序数据的读写权限。更多的信息，请参考[Content Providers](https://developer.android.com/guide/topics/providers/content-providers.html)开发文档


### Using Shared Preferences使用共享参数
---
  
> The [SharedPreferences](https://developer.android.com/reference/android/content/SharedPreferences.html) class provides a general framework that allows you to save and retrieve persistent key-value pairs of primitive data types. You can use [SharedPreferences](https://developer.android.com/reference/android/content/SharedPreferences.html) to save any primitive data: booleans, floats, ints, longs, and strings. This data will persist across user sessions (even if your application is killed).

[SharedPreferences](https://developer.android.com/reference/android/content/SharedPreferences.html)类提供了一个通用的框架允许你存储和读取键值对形式的原始数据类型，你可以在[SharedPreferences](https://developer.android.com/reference/android/content/SharedPreferences.html)里保存诸如booleans, floats, ints, longs, strings的数据类型，这些数据会在用户的会话期间一直存在（即使你的应用被杀死）

#### User Preferences

> Shared preferences are not strictly for saving "user preferences," such as what ringtone a user has chosen. If you're interested in creating user preferences for your application, see [PreferenceActivity](https://developer.android.com/reference/android/preference/PreferenceActivity.html), which provides an Activity framework for you to create user preferences, which will be automatically persisted (using shared preferences).  

严格意义上，Shared preferences并不是用来保存用户配置选项，如果你需要创建一个首选项，请参考[PreferenceActivity](https://developer.android.com/reference/android/preference/PreferenceActivity.html)，它提供了一个框架来创建用户首选项，并且能够自动保存配置信息。

> To get a [SharedPreferences](https://developer.android.com/reference/android/content/SharedPreferences.html) object for your application, use one of two methods:
> 
> * `getSharedPreferences()` - Use this if you need multiple preferences files identified by name, which you specify with the first parameter.
> 
> * `getPreferences()` - Use this if you need only one preferences file for your Activity. Because this will be the only preferences file for your Activity, you don't supply a name.

使用以下的两种方法来得到[SharedPreferences](https://developer.android.com/reference/android/content/SharedPreferences.html)对象：  

`getSharedPreferences()` - 如果你需要多个参数文件，你需要调用这个方法，第一个参数是文件名称  

`getPreferences()` - 如果你只需要一个参数文件，你需要调用这个方法，这个方法不需要制定文件名称，它只会生成一个文件  


> To write values:
> 
> 1. Call edit() to get a [SharedPreferences.Editor](https://developer.android.com/reference/android/content/SharedPreferences.Editor.html).
> 2. Add values with methods such as `putBoolean()` and `putString()`.
> 3. Commit the new values with `commit()`

写入值

1. 调用 edit() 得到一个SharedPreferences.Editor
2. 调用putBoolean()putString()等方法添加值
3. 提交


> To read values, use [SharedPreferences](https://developer.android.com/reference/android/content/SharedPreferences.html) methods such as `getBoolean()` and `getString()`.

调用[SharedPreferences](https://developer.android.com/reference/android/content/SharedPreferences.html)的方法例如`getBoolean()` `getString()` 来读取值

> Here is an example that saves a preference for silent keypress mode in a calculator:

一个计算器关闭按键音的例子

    public class Calc extends Activity {
    public static final String PREFS_NAME = "MyPrefsFile";
    
    @Override
    protected void onCreate(Bundle state){
       super.onCreate(state);
       . . .
    
       // Restore preferences
       SharedPreferences settings = getSharedPreferences(PREFS_NAME, 0);
       boolean silent = settings.getBoolean("silentMode", false);
       setSilent(silent);
    }
    
    @Override
    protected void onStop(){
       super.onStop();
    
      // We need an Editor object to make preference changes.
      // All objects are from android.context.Context
      SharedPreferences settings = getSharedPreferences(PREFS_NAME, 0);
      SharedPreferences.Editor editor = settings.edit();
      editor.putBoolean("silentMode", mSilentMode);
    
      // Commit the edits!
      editor.commit();
    }
    }

### Using the Internal Storage使用内部存储器  
---
> You can save files directly on the device's internal storage. By default, files saved to the internal storage are private to your application and other applications cannot access them (nor can the user). When the user uninstalls your application, these files are removed.

你可以在设备内部存储器里直接保存文件，默认情况下，应用程序存储在内部存储器里的数据是私有的，其他应用或用户是无法访问的。当用户卸载应用时这些文件会被移除

> To create and write a private file to the internal storage:
> 
> 1. Call `openFileOutput()` with the name of the file and the operating mode. This returns a [FileOutputStream](https://developer.android.com/reference/java/io/FileOutputStream.html).
> 2. Write to the file with `write()`.
> 3. Close the stream with `close()`.
> 
> For example:

在内部存储器创建并读写

1. 使用`openFileOutput()`方法，参数是文件名称和操作模式，返回值是一个[FileOutputStream](https://developer.android.com/reference/java/io/FileOutputStream.html)
2. 调用`write()`写入文件
3. 调用`close()`关闭流

例如:

    String FILENAME = "hello_file";
    String string = "hello world!";
    
    FileOutputStream fos = openFileOutput(FILENAME, Context.MODE_PRIVATE);
    fos.write(string.getBytes());
    fos.close();

> 
> `MODE_PRIVATE` will create the file (or replace a file of the same name) and make it private to your application. Other modes available are: `MODE_APPEND`, `MODE_WORLD_READABLE`, and `MODE_WORLD_WRITEABLE`.

MODE\_PRIVATE模式创建（或替换）文件，并使其私有化，其他的模式有: MODE\_APPEND, MODE\_WORLD\_READABLE, 和MODE\_WORLD\_WRITEABLE.

> To read a file from internal storage:
> 
> 1. Call `openFileInput()` and pass it the name of the file to read. This returns a [FileInputStream](https://developer.android.com/reference/java/io/FileInputStream.html).
> 2. Read bytes from the file with `read()`.
> 3. Then close the stream with `close()`.

从内存器中读取文件

1. 调用openFileInput() 并指定读取的文件名称. 返回值是一个FileInputStream.
2. 调用read()读取字节
3. 使用 close().关闭流


> Tip: If you want to save a static file in your application at compile time, save the file in your project `res/raw/directory`. You can open it with `openRawResource()`, passing the `R.raw.<filename>` resource ID. This method returns an InputStream that you can use to read the file (but you cannot write to the original file).

提示: 如果你想在编译时保存一个静态文件, 你需要包文件保存在 `res/raw/directory`文件夹下. 使用`openRawResource()`方法打开, 把` R.raw.<filename>` 资源ID当作参数传递给它. 这个方法会返回一个InputStream来让你读取文件(但是你不能对这个原始文件进行写入操作).

### Saving cache files保存缓存文件

> If you'd like to cache some data, rather than store it persistently, you should use `getCacheDir()` to open a Filethat represents the internal directory where your application should save temporary cache files.
> 
> When the device is low on internal storage space, Android may delete these cache files to recover space. However, you should not rely on the system to clean up these files for you. You should always maintain the cache files yourself and stay within a reasonable limit of space consumed, such as 1MB. When the user uninstalls your application, these files are removed.


如果你想缓存一些数据,你可以使用getCacheDir() 来打开一个代表内存器中缓存文件夹的File，你可以在这个文件下保存临时的应用缓存数据.  

当设备空间不足时，系统会删除这些缓存的文件。然而，你不能依赖于系统去清除这些文件，你应该手动管理这些缓存文件让它们处在一个合适的大小范围内，比如1MB，当你卸载应用时这些文件也会被删除

### Other useful methods其他有用的方法

> `getFilesDir()`  
> 　　Gets the absolute path to the filesystem directory where your internal files are saved.  
> 　　得到文件的绝对路径  
> `getDir()`  
> 　　Creates (or opens an existing) directory within your internal storage space.  
> 　　创建或打开文件夹  
> `deleteFile()`  
> 　　Deletes a file saved on the internal storage.  
> 　　删除文件  
> `fileList()`  
> 　　Returns an array of files currently saved by your application.  
> 　　返回应用的文件数组　　




### Using the External Storage使用外部存储器
---

> Every Android-compatible device supports a shared "external storage" that you can use to save files. This can be a removable storage media (such as an SD card) or an internal (non-removable) storage. Files saved to the external storage are world-readable and can be modified by the user when they enable USB mass storage to transfer files on a computer.

每一个安卓设置都支持一个共享的外部存储器，这可以是一个可移除的存储介质（SDCard）或一个不可移除的内存卡。当用户将设备当作一个U盘连接到电脑上时，保存在外部存储器上的文件可以被用户读取的并修改的
> 
> Caution: External storage can become unavailable if the user mounts the external storage on a computer or removes the media, and there's no security enforced upon files you save to the external storage. All applications can read and write files placed on the external storage and the user can remove them.

警告: 如果用户将外部存储器挂载到计算机上，或者移除了这个介质，它将变得不可用, 你保存在外部存储器上的文件也没有强制安全权限. 任何应用都能读写在外部存储器上的文件并且用户可以删除它们

#### Getting access to external storage得到外部存储器的权限  

> In order to read or write files on the external storage, your app must acquire the `READ_EXTERNAL_STORAGE` or `WRITE_EXTERNAL_STORAGE` system permissions. For example:

为了得到外部存储器的读写权限, 你的应用必须配置`READ_EXTERNAL_STORAGE`或者`WRITE_EXTERNAL_STORAGE`系统权限，例如：

    <manifest ...>
    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
    ...
    </manifest>

> If you need to both read and write files, then you need to request only the `WRITE_EXTERNAL_STORAGE` permission, because it implicitly requires read access as well.

如果你需要可读并可写的权限, 你只需要请求`WRITE_EXTERNAL_STORAGE` 权限，因为它也包含了可读权限

> Note: Beginning with Android 4.4, these permissions are not required if you're reading or writing only files that are private to your app. For more information, see the section below about [saving files that are app-private](https://developer.android.com/intl/zh-cn/guide/topics/data/data-storage.html#AccessingExtFiles).

注意: 从android4.4开始, 在读取私有文件的时候，这些权限就无需声明了. 更多信息请参考[saving files that are app-private](https://developer.android.com/intl/zh-cn/guide/topics/data/data-storage.html#AccessingExtFiles).

#### Checking media availability检查介质是否可用

> Before you do any work with the external storage, you should always call `getExternalStorageState()` to check whether the media is available. The media might be mounted to a computer, missing, read-only, or in some other state. For example, here are a couple methods you can use to check the availability:

你在外部存储器上开始操作之前, 你应该使用 `getExternalStorageState()` 方法来确认设备是否可用. 媒介有可能被挂载在pc上，丢失，或只读. 这里有一些方法用来确认媒介是否可以使用:

    /* Checks if external storage is available for read and write */
    public boolean isExternalStorageWritable() {
    String state = Environment.getExternalStorageState();
    if (Environment.MEDIA_MOUNTED.equals(state)) {
    return true;
    }
    return false;
    }
    
    /* Checks if external storage is available to at least read */
    public boolean isExternalStorageReadable() {
    String state = Environment.getExternalStorageState();
    if (Environment.MEDIA_MOUNTED.equals(state) ||
    Environment.MEDIA_MOUNTED_READ_ONLY.equals(state)) {
    return true;
    }
    return false;
    }

> The `getExternalStorageState()` method returns other states that you might want to check, such as whether the media is being shared (connected to a computer), is missing entirely, has been removed badly, etc. You can use these to notify the user with more information when your application needs to access the media.

使用 `getExternalStorageState()` 方法返回媒介的状态, 比如介质是否被共享,是否丢失,是否被不恰当的方式移除,等等. 你能在应用需要访问介质的时候用这些状态通知用户.

#### Saving files that can be shared with other apps存储能与其他应用共享的文件

> Hiding your files from the Media Scanner
> 
> Include an empty file named.nomedia in your external files directory (note the dot prefix in the filename). This prevents media scanner from reading your media files and providing them to other apps through the MediaStore content provider. However, if your files are truly private to your app, you should save them in an app-private directory.
> Generally, new files that the user may acquire through your app should be saved to a "public" location on the device where other apps can access them and the user can easily copy them from the device. When doing so, you should use to one of the shared public directories, such as `Music/`, `Pictures/`, and `Ringtones/`.

通常来说, 这些新文件需要保存在一个公共的位置，这样用户或其他的应用可以获取并使用它们.当你这么做的时候你需要用到一些公共的文件夹例如 `Music/`, `Pictures/`, `Ringtones/`.

> To get a File representing the appropriate public directory, callgetExternalStoragePublicDirectory(), passing it the type of directory you want, such as `DIRECTORY_MUSIC`,`DIRECTORY_PICTURES`, `DIRECTORY_RINGTONES`, or others. By saving your files to the corresponding media-type directory, the system's media scanner can properly categorize your files in the system (for instance, ringtones appear in system settings as ringtones, not as music).

使用 `getExternalStoragePublicDirectory()` 方法，给它传递参数表示你需要的类型, 例如`DIRECTORY_MUSIC`,`DIRECTORY_PICTURES`,` DIRECTORY_RINGTONES` 来得到代表这些公共文件夹的File对象。 系统媒体扫描器能将你所保存的文件在系统里进行合理的分类，如果你将文件保存在正确的位置 (例如，铃声会在设置里显示为铃声，而不是音乐).

> For example, here's a method that creates a directory for a new photo album in the public pictures directory:

这里是一个在公共图片文件夹里创建相册的例子

    public File getAlbumStorageDir(String albumName) {
    // Get the directory for the user's public pictures directory.
    File file = new File(Environment.getExternalStoragePublicDirectory(
    Environment.DIRECTORY_PICTURES), albumName);
    if (!file.mkdirs()) {
    Log.e(LOG_TAG, "Directory not created");
    }
    return file;
    }

#### Saving files that are app-private将文件保存为应用私有类型

> If you are handling files that are not intended for other apps to use (such as graphic textures or sound effects used by only your app), you should use a private storage directory on the external storage by `callinggetExternalFilesDir()`. This method also takes a type argument to specify the type of subdirectory (such as DIRECTORY_MOVIES). If you don't need a specific media directory, pass `null` to receive the root directory of your app's private directory.

如果你管理的文件不打算给其他的应用使用 (例如图形纹理或音效), 你需要调用`getExternalFilesDir()`方法来在外部存储器中使用一个私有的文件夹.这个方法也需要一个type 参数来指定子文件夹的类型. 如果你不需要指定子文件夹的类型，传递一个`null`给这个方法，它会返回私有文件夹的根目录.

> Beginning with Android 4.4, reading or writing files in your app's private directories does not require the `READ_EXTERNAL_STORAGE` or `WRITE_EXTERNAL_STORAGE` permissions. So you can declare the permission should be requested only on the lower versions of Android by adding the maxSdkVersion attribute:

从Android 4.4开始, 读写外部存储器的私有文件不需要配置`READ_EXTERNAL_STORAGE` 或 `WRITE_EXTERNAL_STORAGE` 权限. 因此你仅仅需要为低版本的系统指定这些权限:

    <manifest ...>
    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"
     android:maxSdkVersion="18" />
    ...
    </manifest>

> Note: When the user uninstalls your application, this directory and all its contents are deleted. Also, the system media scanner does not read files in these directories, so they are not accessible from the [MediaStore](https://developer.android.com/reference/android/provider/MediaStore.html) content provider. As such, you should not use these directories for media that ultimately belongs to the user, such as photos captured or edited with your app, or music the user has purchased with your app—those files should be [saved in the public directories](https://developer.android.com/intl/zh-cn/guide/topics/data/data-storage.html#SavingSharedFiles).

注意: 当你卸载应用时, 这些文件夹也会被删除. 另外, 系统媒体扫描器不会扫描这些文件夹,因此[MediaStore](https://developer.android.com/reference/android/provider/MediaStore.html)内容提供者也没有访问权限. 同样的, 你也不应该使用这些文件夹来存储最终属于用户媒体文件, 例如当前应用拍摄或编辑的照片, 或者用户通过你的应用购买的音乐，这些文件应该[存储在公共文件夹下](https://developer.android.com/intl/zh-cn/guide/topics/data/data-storage.html#SavingSharedFiles).

> Sometimes, a device that has allocated a partition of the internal memory for use as the external storage may also offer an SD card slot. When such a device is running Android 4.3 and lower, the `getExternalFilesDir()`method provides access to only the internal partition and your app cannot read or write to the SD card. Beginning with Android 4.4, however, you can access both locations by calling `getExternalFilesDirs()`, which returns aFile array with entries each location. The first entry in the array is considered the primary external storage and you should use that location unless it's full or unavailable. If you'd like to access both possible locations while also supporting Android 4.3 and lower, use the [support library's](https://developer.android.com/tools/support-library/index.html) static method,`ContextCompat.getExternalFilesDirs()`. This also returns a `File` array, but always includes only one entry on Android 4.3 and lower.

有些时候, 一些分配内部存储器的分区当作外部存储器来使用的设备也提供了SD卡插槽.这些设备在运行Android 4.3或更低的版本时, `getExternalFilesDir()`方法提供只有内部存储器的访问接口，这样你的应用无法在SD卡上读写. 而从Android 4.4开始, 使用`getExternalFilesDirs()`能访问两种位置的外部存储器了，返回一个包含了每一个入口位置的File数组. 这个数组里的第一个入口被当作是优先使用的外部存储器，你应该使用这部分的外部存储器除非空间已满或者不可使用.如果你在Android 4.3或者更低版本的系统上需要访问这两种位置, 使用[support library's ](https://developer.android.com/tools/support-library/index.html)静态方法`ContextCompat.getExternalFilesDirs()`.它返回的也是一个`File`数组, 但是永远只包含了一个入口.

> **Caution** Although the directories provided by `getExternalFilesDir()` and `getExternalFilesDirs()` are not accessible by the [MediaStore](#) content provider, other apps with the `READ_EXTERNAL_STORAGE` permission can access all files on the external storage, including these. If you need to completely restrict access for your files, you should instead write your files to the [internal storage](https://developer.android.com/intl/zh-cn/guide/topics/data/data-storage.html#filesInternal).

注意 虽然使用 `getExternalFilesDir() ``getExternalFilesDirs()` 提供的文件夹无法被 [MediaStore](#) 内容提供者所访问, 但是其他配置了`READ_EXTERNAL_STORAGE`权限的应用能访问在外部存储器上的所有文件,也包含了这些文件夹. 如果你想要完全限制访问这些文件夹, 你需要将它们存储在内部存储器上.

#### Saving cache files保存缓存文件

> To open a `File` that represents the external storage directory where you should save cache files, call `getExternalCacheDir()`. If the user uninstalls your application, these files will be automatically deleted.
> 
> Similar to `ContextCompat.getExternalFilesDirs()`, mentioned above, you can also access a cache directory on a secondary external storage (if available) by calling 
> `ContextCompat.getExternalCacheDirs()`.

使用`getExternalCacheDir()`来获得代表外部存储器文件夹的`File`对象来保存你的缓存文件,如果用户卸载了应用，这些文件也会被彻底删除.

与`ContextCompat.getExternalFilesDirs()`方法类似,你也可以使用`ContextCompat.getExternalCacheDirs()`方法访问第二个外部存储器上的缓存文件夹

> Tip: To preserve file space and maintain your app's performance, it's important that you carefully manage your cache files and remove those that aren't needed anymore throughout your app's lifecycle.

提示: 为了维持文件空间保持应用的体验, 细心的管理你的缓存文件并及时删除应用不需要的缓存文件是非常重要的.

### Using Databases使用数据库
---
> Android provides full support for [SQLite](http://www.sqlite.org/) databases. Any databases you create will be accessible by name to any class in the application, but not outside the application.  
> 
The recommended method to create a new SQLite database is to create a subclass of [SQLiteOpenHelper](https://developer.android.com/reference/android/database/sqlite/SQLiteOpenHelper.html) and override the `onCreate()` method, in which you can execute a SQLite command to create tables in the database. For example:  

Android提供了对SQLite数据库的全面支持. 应用中任何类都可以访问你创建的任何数据库,应用外则不行.

推荐的方式是创建一个[SQLiteOpenHelper](https://developer.android.com/reference/android/database/sqlite/SQLiteOpenHelper.html)的子类并重写`onCreate()`方法来创建一个数据, 这个方法里可以执行SQLite命令来创建表. 例如:

    public class DictionaryOpenHelper extends SQLiteOpenHelper {
    
    private static final int DATABASE_VERSION = 2;
    private static final String DICTIONARY_TABLE_NAME = "dictionary";
    private static final String DICTIONARY_TABLE_CREATE =
    "CREATE TABLE " + DICTIONARY_TABLE_NAME + " (" +
    KEY_WORD + " TEXT, " +
    KEY_DEFINITION + " TEXT);";
    
    DictionaryOpenHelper(Context context) {
    super(context, DATABASE_NAME, null, DATABASE_VERSION);
    }
    
    @Override
    public void onCreate(SQLiteDatabase db) {
    db.execSQL(DICTIONARY_TABLE_CREATE);
    }
    }

> You can then get an instance of your [SQLiteOpenHelper](https://developer.android.com/reference/android/database/sqlite/SQLiteOpenHelper.html) implementation using the constructor you've defined. To write to and read from the database, call `getWritableDatabase()` and `getReadableDatabase()`, respectively. These both return a [SQLiteDatabase](https://developer.android.com/reference/android/database/sqlite/SQLiteOpenHelper.html) object that represents the database and provides methods for SQLite operations.

你能使用你定义的构造函数得到一个 [SQLiteOpenHelper](https://developer.android.com/reference/android/database/sqlite/SQLiteOpenHelper.html) 接口实例. 使用 `getWritableDatabase()`和`getReadableDatabase()`方法来读写数据库 。它们同样返回代表数据库并且提供对SQLite进行操作的 [SQLiteOpenHelper](https://developer.android.com/reference/android/database/sqlite/SQLiteOpenHelper.html) 对象.

> Android does not impose any limitations beyond the standard SQLite concepts. We do recommend including an autoincrement value key field that can be used as a unique ID to quickly find a record. This is not required for private data, but if you implement acontent provider, you must include a unique ID using theBaseColumns._ID constant.  
> You can execute SQLite queries using the `SQLiteDatabasequery()` methods, which accept various query parameters, such as the table to query, the projection, selection, columns, grouping, and others. For complex queries, such as those that require column aliases, you should use [SQLiteQueryBuilder](https://developer.android.com/reference/android/database/sqlite/SQLiteQueryBuilder.html), which provides several convienent methods for building queries.

你可以使用`SQLiteDatabasequery()`方法来执行查询操作, 这个方法接收各种查询参数, 例如表查询，映射关系等等. 更复杂的查询,你需要使用[SQLiteQueryBuilder](https://developer.android.com/reference/android/database/sqlite/SQLiteQueryBuilder.html),该类提供了几个更为简便的方法来建立查询.

> Every SQLite query will return a [Cursor](https://developer.android.com/reference/android/database/Cursor.html) that points to all the rows found by the query. The [Cursor](https://developer.android.com/reference/android/database/Cursor.html) is always the mechanism with which you can navigate results from a database query and read rows and columns.

每一个SQLite 查询都会返回[Cursor](https://developer.android.com/reference/android/database/Cursor.html)来指向所有的查询结果. [Cursor](https://developer.android.com/reference/android/database/Cursor.html)是一个能定位查询结果的机制.

> For sample apps that demonstrate how to use SQLite databases in Android, see the Note Pad and Searchable Dictionary applications.

关于演示安卓上如何使用SQLite数据的示例, 请参考 [Note Pad](https://developer.android.com/resources/samples/NotePad/index.html) 和[Searchable Dictionary](https://developer.android.com/resources/samples/SearchableDictionary/index.html).

#### Database debugging数据库调试

> The Android SDK includes a sqlite3 database tool that allows you to browse table contents, run SQL commands, and perform other useful functions on SQLite databases. See [Examining sqlite3 databases from a remote shell](https://developer.android.com/tools/help/adb.html#sqlite) to learn how to run this tool.

​Android SDK 包含了sqlite3数据库工具，允许开发人员浏览表内容,运行SQL命令,执行其他有用的功能. 学习如何使用这个工具请参考[Examining sqlite3 databases from a remote shell](https://developer.android.com/tools/help/adb.html#sqlite).

### Using a Network Connection使用网络连接
---
> You can use the network (when it's available) to store and retrieve data on your own web-based services. To do network operations, use classes in the following packages:

你能使用网络在你的web服务器上存取数据.使用下面包中的类来进行网络操作:

    java.net.*  
    android.net.*







