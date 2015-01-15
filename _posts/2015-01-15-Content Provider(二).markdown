---
layout: post
title:  "Content Provider(二)"
date:   2015-01-15 17:00:30
categories: jekyll update
---
这篇文章主要介绍有：

1. ContentProvider如何工作
2. 用于检索ContentProvider的API
3. 用于插入，更新，删除数据的API
4. 其他便于与ContentProvider工作的API

# **概述** #
## 访问provider ##
访问provider必须提供一个ContentResolver，ContentResolver会同等地调用provider提供的“CRUD”方法
> 注意：要访问provider，必须提供某些权限

样例：

    mCursor = getContentResolver().query(
    	UserDictionary.Words.CONTENT_URI,    //提供表格URI
    	mProjection,					     // 要检索的列
    	mSelectionClause,					 // 检索条件
    	mSeletionArgs,						 // 检索条件参数
    	mSortOrder,							 // 返回结果顺序
    );

## Content URIs ##
Content URI是用来标示一个Content Provider的中的表格的，他不仅包含一个authority(确定是哪个provider)还有一个path（确定是哪个表），通过这种解析方法才能找到要处理的表格。例如：

    content://user_dictionary/words

这个uri中user_dictionary代表authority，words代表表格，而前面的content://(这是scheme)是必须有的代表了这是一个content Uri。

> 注意：Uri和Uri.Builder类提供了便捷的方法来通过字符串构造Uri对象，ContentUris也提供了便捷方法用来在Uri上添加值，比如withAppendedId()用来添加id

# 从provider中检索数据 #
建议在检索数据时不要在UI主线程中执行，而要再开辟一个新线程来执行。如果在主线程中使用的话，要使用CursorLoader类。

检索数据步骤：

1. 申请访问权限
	1. 针对不同的provider需要不同的权限
2. 写访问代码

	 	String[] mProjection = {
    		UserDictionary.Words._ID,
			UserDictionary.Words.WORD,
			UserDictionary.Words.LOCALE
    	};
		String mSelectionClause = null;
		String[] mSelectionArgs = {""};

**防止恶意Sql注入**

如果直接使用字符串拼接来完成sql语句，如

    String mSelectionClause = "var = "+mUserInput;

用户可能写入“nothing;Drop Table *;”这样就会删除所有相关数据，而使用

    String mSelectionClause = "var = ?";

通过？替代就没有sql注入问题


**显示查询结果**

ContentResolver提供的查询结果为Cursor对象，他能够随机访问返回的数据，如果查不到数据curosr.getCount()返回为0，如果出现错误则返回为null或者抛出Exception，系统默认中通过SimpleCursorAdapter来展示数据，是一个非常好的方式。代码如下
	
	// 指定要查询的列
    String[] mWordListColumns = {
    	UserDictionary.Words.WORD,
    	UserDictionary.Words.LOCALE,
    };
	// 指定通过那些组件显示
    int[] mWordListItems ={R.id.dictWord,R.id.locale};
    mCursorAdapter = new SimpleCursorAdapter(){
	    getApplicationContext,
	    R.layout.wordlistrow,   // 每一行的布局
	    mCursor,
	    mWordListColumns,
	    mWordListItems,
	    0						// 通常没啥用
    };
	mWordList.setAdapter(mCursorAdapter);

> 这种使用方式必须包含_ID列，即使不显示出来也必须要有

## **插入数据** ##

    ContentValues mNewValues = new ContentValues();
    
    /*
     * Sets the values of each column and inserts the word. The arguments to the "put"
     * method are "column name" and "value"
     */
    mNewValues.put(UserDictionary.Words.APP_ID, "example.user");
    mNewValues.put(UserDictionary.Words.LOCALE, "en_US");
    mNewValues.put(UserDictionary.Words.WORD, "insert");
    mNewValues.put(UserDictionary.Words.FREQUENCY, "100");
    
    mNewUri = getContentResolver().insert(
    UserDictionary.Word.CONTENT_URI,   // the user dictionary content URI
    mNewValues  // the values to insert
    );

插入成功后会返回函数插入数据ID的新的Uri

## **更新数据** ##

    // Defines an object to contain the updated values
    ContentValues mUpdateValues = new ContentValues();
    
    // Defines selection criteria for the rows you want to update
    String mSelectionClause = UserDictionary.Words.LOCALE +  "LIKE ?";
    String[] mSelectionArgs = {"en_%"};
    
    // Defines a variable to contain the number of updated rows
    int mRowsUpdated = 0;
    
    ...
    
    /*
     * Sets the updated value and updates the selected words.
     */
    mUpdateValues.putNull(UserDictionary.Words.LOCALE);
    
    mRowsUpdated = getContentResolver().update(
    UserDictionary.Words.CONTENT_URI,   // the user dictionary content URI
    mUpdateValues   // the columns to update
    mSelectionClause// the column to select on
    mSelectionArgs  // the value to compare to
    );

## **删除数据** ##

    // Defines selection criteria for the rows you want to delete
    String mSelectionClause = UserDictionary.Words.APP_ID + " LIKE ?";
    String[] mSelectionArgs = {"user"};
    
    // Defines a variable to contain the number of rows deleted
    int mRowsDeleted = 0;
    
    ...
    
    // Deletes the words that match the selection criteria
    mRowsDeleted = getContentResolver().delete(
    UserDictionary.Words.CONTENT_URI,   // the user dictionary content URI
    mSelectionClause// the column to select on
    mSelectionArgs  // the value to compare to
    );
 
## **其他访问provider方式** ##

- 批处理：利用ContentProviderOperation类
- 异步访问：开辟新线程访问
- 通过intent访问数据：通过发送intent给应用程序来间接访问数据

## **MIME类型参考** ##

provider可以返回标准的或自定义的MIME类型
MIME类型格式：

    type/subtype

自定义的MIME类型也叫做"vendor-specific"MEMI类型有更加复杂的类型与子类型,其类型通常为

    vnd.android.cursor.dir
用于多行，或者为

	vnd.android.cursor.item
用于单行。
对于子类型通常是provider自己定义的，对于Android自建的provider通常含有比较简单子类型，如：

    vnd.android.cursor.item/phone_v2

