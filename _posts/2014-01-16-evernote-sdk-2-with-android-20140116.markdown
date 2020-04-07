---
layout: post
layout: post
author: kkdai
comments: true
date: 2014-01-16 13:45:37+00:00
slug: evernote-sdk-2-with-android-20140116
title: Evernote SDK (2) with Android 2014/01/16
wordpress_id: 1296
categories:
- Android安卓學習心得
- 研討會心得
---




Slide: [http://goo.gl/RPbGVn](http://goo.gl/RPbGVn)




 




About Evernote SDK API trick or experience discussion: (for iOS/Android.)






  * **findNote **— [http://dev.evernote.com/doc/reference/NoteStore.html#Fn_NoteStore_findNotes](http://dev.evernote.com/doc/reference/NoteStore.html#Fn_NoteStore_findNotes)


  * 


    * It must 


    * It is depressed, because it has performance issue to get all notes entity (include all attachment) 


    * 


      * But it still could use for now. 





    * It alway need to assign number of notes, but you could recessive to get all notes.


    * Currently Evernote official will suggest to use “findNotesMetadata” and “getNote”


    * if you need to ordering your result, using filter.





  * **findNotesMetadata **— [http://dev.evernote.com/doc/reference/NoteStore.html#Fn_NoteStore_findNotesMetadata](http://dev.evernote.com/doc/reference/NoteStore.html#Fn_NoteStore_findNotesMetadata)


  * 


    * Please note: findNote performance is still better  than findNotesMetadata + getNote 





  * **getNote **— ([http://dev.evernote.com/doc/reference/NoteStore.html#Fn_NoteStore_getNote](http://dev.evernote.com/doc/reference/NoteStore.html#Fn_NoteStore_getNote))


  * 


    * 

    
    getNote(<code>string</code> authenticationToken,
                       <code><a style="color:#222222;text-decoration:none;" href="http://dev.evernote.com/doc/reference/Types.html#Typedef_Guid">Types.Guid</a></code> guid,
                       <code>bool</code> withContent,
                       <code>bool</code> withResourcesData,
                       <code>bool</code> withResourcesRecognition,
    



    
    <code>bool</code> withResourcesAlternateData)
    





    * withResourcesRecognition —   



    * 


      * It help to get back image recognition(evernote specific), but it will depends on priority (pay account first)





    * withContent — 


    * 


      * content is HTML data using <en-note> HTML data </en-note>, you could easily to put <en-note> HTML data </en-note> to browser.





    * withResourcesData — 


    * 


      * For attachment or image <en-media> it will contain a hash code for your to reserve file .








  * **createNoteBook **— [http://dev.evernote.com/doc/reference/NoteStore.html#Fn_NoteStore_createNotebook](http://dev.evernote.com/doc/reference/NoteStore.html#Fn_NoteStore_createNotebook)


  * 


    * NoteBook “setName" could not duplicate.


    * 


      * It could be easily to getNoteBook first before you try to createNoteBook.








  * **createNote **—  [http://dev.evernote.com/doc/reference/NoteStore.html#Fn_NoteStore_createNote](http://dev.evernote.com/doc/reference/NoteStore.html#Fn_NoteStore_createNote)


  * 


    * createNote using “setTitle” not setName. :)


    * Need to add <en-note> for your HTML before you create note, using EvernoteUtil.createEnMediaTag 


    * EML (Evernote Metadata Language)


    * 


      * Need take care <en-media> if you need using browser to display it.


      * <div> might broken, because any WYSIWYG editor has the same issue.








  * **Image handle in Android**


  * 


    * Need using InputStream to handle it locally before you add to note.





