# NotePad
期中实验
1.添加笔记查询功能（根据标题查询）
```
private void SearchView(){
        searchView=findViewById(R.id.sv);
        searchView.onActionViewExpanded();
        searchView.setQueryHint("搜索笔记");
        searchView.setSubmitButtonEnabled(true);
        searchView.setOnQueryTextListener(new SearchView.OnQueryTextListener() {
            @Override
            public boolean onQueryTextSubmit(String s) {
                return false;
            }

            @Override
            public boolean onQueryTextChange(String s) {
                if(!s.equals("")){
                    String selection=NotePad.Notes.COLUMN_NAME_TITLE+" GLOB '*"+s+"*'";
                    updatecursor = getContentResolver().query(
                            getIntent().getData(),            // Use the default content URI for the provider.
                            PROJECTION,                       // Return the note ID and title for each note.
                            selection,                             // No where clause, return all records.
                            null,                             // No where clause, therefore no where column values.
                            NotePad.Notes.DEFAULT_SORT_ORDER  // Use the default sort order.
                    );
                    if(updatecursor.moveToNext())
                        Log.i("daawdwad",selection);
                }
               else {
                    updatecursor = getContentResolver().query(
                            getIntent().getData(),            // Use the default content URI for the provider.
                            PROJECTION,                       // Return the note ID and title for each note.
                            null,                             // No where clause, return all records.
                            null,                             // No where clause, therefore no where column values.
                            NotePad.Notes.DEFAULT_SORT_ORDER  // Use the default sort order.
                    );
                }
                adapter.swapCursor(updatecursor);

               // adapter.notifyDataSetChanged();
                return false;
            }
        });
    }
```
```
    <android.support.v7.widget.SearchView
        android:id="@+id/sv"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        >
```
   #### 2.更改记事本的背景
   修改了所有显示页面的背景。
   ```
   <LinearLayout
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:background="@drawable/a"
    xmlns:android="http://schemas.android.com/apk/res/android">
    <android.support.v7.widget.SearchView
        android:id="@+id/sv"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        >
    </android.support.v7.widget.SearchView>
    <ListView
        android:id="@+id/tv"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:fastScrollEnabled="true"
        android:listSelector="@drawable/timer_list_selector"
        android:cacheColorHint="@android:color/transparent"
        android:scrollingCache="false"
        android:fadingEdge="none"
        android:divider="#00000000"/>
</LinearLayout>
```

```
<?xml version="1.0" encoding="utf-8"?>
<!-- Copyright (C) 2010 The Android Open Source Project

     Licensed under the Apache License, Version 2.0 (the "License");
     you may not use this file except in compliance with the License.
     You may obtain a copy of the License at
  
          http://www.apache.org/licenses/LICENSE-2.0
  
     Unless required by applicable law or agreed to in writing, software
     distributed under the License is distributed on an "AS IS" BASIS,
     WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
     See the License for the specific language governing permissions and
     limitations under the License.
-->
<view xmlns:android="http://schemas.android.com/apk/res/android"
    class="com.example.administrator.notepad.NoteEditor$LinedEditText"
    android:id="@+id/note"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:background="@drawable/b"
    android:padding="5dp"
    android:scrollbars="vertical"
    android:fadingEdge="vertical"
    android:gravity="top"
    android:textSize="22sp"
    android:inputType="textCapSentences"
/>
<!--<EditText
    android:id="@+id/note"
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_height="match_parent"
    android:layout_width="match_parent"/>-->
```

```
<?xml version="1.0" encoding="utf-8"?>
<!-- Copyright (C) 2010 The Android Open Source Project

     Licensed under the Apache License, Version 2.0 (the "License");
     you may not use this file except in compliance with the License.
     You may obtain a copy of the License at
  
          http://www.apache.org/licenses/LICENSE-2.0
  
     Unless required by applicable law or agreed to in writing, software
     distributed under the License is distributed on an "AS IS" BASIS,
     WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
     See the License for the specific language governing permissions and
     limitations under the License.
-->

<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android" 
  	android:layout_width="wrap_content" 
    android:layout_height="wrap_content"
    android:orientation="vertical"
    android:background="@drawable/d"
    android:paddingLeft="6dip"
    android:paddingRight="6dip"
    android:paddingBottom="3dip">
   					  
    <TextView android:id="@+id/title"
        android:maxLines="1"
        android:layout_marginTop="2dp"
        android:layout_marginBottom="15dp"
        android:layout_width="wrap_content"
      	android:ems="25"
        android:layout_height="wrap_content"
        android:inputType="textCapSentences|textAutoCorrect"
        android:scrollHorizontally="true" />
    <EditText android:id="@+id/title1"
        android:maxLines="1"
        android:layout_marginTop="2dp"
        android:layout_marginBottom="15dp"
        android:layout_width="wrap_content"
        android:ems="25"
        android:layout_height="wrap_content"
        android:inputType="textCapSentences|textAutoCorrect"
        android:scrollHorizontally="true" />
   		
    <Button android:id="@+id/ok"
        android:layout_width="wrap_content" 
        android:layout_height="wrap_content" 
        android:layout_gravity="right"
        android:text="@string/button_ok"
        android:onClick="onClickOk" />
   		
</LinearLayout>

```


#### 3.笔记内容字体颜色和大小更改的功能。
实现代码如下：
```
 public boolean onOptionsItemSelected(MenuItem item) {
        // Handle all of the possible menu actions.
        switch (item.getItemId()) {
        case R.id.menu_save:
            String text = mText.getText().toString();
            updateNote(text, null);
            finish();
            break;
        case R.id.menu_delete:
            deleteNote();
            finish();
            break;
        case R.id.menu_revert:
            cancelNote();
            break;
            case R.id.red_font:
                mText.setTextColor(Color.RED);
                break;
            case R.id.black_font:
                mText.setTextColor(Color.BLACK);
            case R.id.font_10:
                mText.setTextSize(20);
                break;
            case R.id.font_16:
                mText.setTextSize(32);
                break;
            case R.id.font_20:
                mText.setTextSize(40);
                break;
        }
        return super.onOptionsItemSelected(item);
    }
    ```
#### 4.记录记事本时间
    ```
public void testWriteDataToPipe() throws FileNotFoundException {

        // A string array to hold the incoming data
        String[] inputData = {"","",""};

        // A URI for a note ID.
        Uri noteIdUri;

        // A Cursor to contain the retrieved note.
        Cursor noteIdCursor;

        // An AssetFileDescriptor for the pipe.
        AssetFileDescriptor noteIdAssetDescriptor;

        // The ParcelFileDescriptor in the AssetFileDescriptor
        ParcelFileDescriptor noteIdParcelDescriptor;

        // Inserts test data into the provider.
        insertData();

        // Creates note ID URI for a note that should now be in the provider.
        noteIdUri = ContentUris.withAppendedId(
                NotePad.Notes.CONTENT_ID_URI_BASE,  // The base pattern for a note ID URI
                1                                   // Sets the URI to point to record ID 1 in the
                                                    // provider
        );

        // Gets a Cursor for the note.
        noteIdCursor = mMockResolver.query(
                noteIdUri,  // the URI for the note ID we want to retrieve
                null,       // no projection, retrieve all the columns
                null,       // no WHERE clause
                null,       // no WHERE arguments
                null        // default sort order
        );

        // Checks that the call worked.
        // a) Checks that the cursor is not null
        // b) Checks that it contains a single record
        assertNotNull(noteIdCursor);
        assertEquals(1,noteIdCursor.getCount());

        // Opens the pipe that will contain the data.
        noteIdAssetDescriptor = mMockResolver.openTypedAssetFileDescriptor(
                noteIdUri,        // the URI of the note that will provide the data
                MIME_TYPE_TEXT,   // the "text/plain" MIME type
                null              // no other options
        );

        // Checks that the call worked.
        // a) checks that the AssetFileDescriptor is not null
        // b) gets its ParcelFileDescriptor
        // c) checks that the ParcelFileDescriptor is not null
        assertNotNull(noteIdAssetDescriptor);
        noteIdParcelDescriptor = noteIdAssetDescriptor.getParcelFileDescriptor();
        assertNotNull(noteIdParcelDescriptor);

        // Gets a File Reader that can read the pipe.
        FileReader fIn = new FileReader(noteIdParcelDescriptor.getFileDescriptor());

        // Gets a buffered reader wrapper for the File Reader. This allows reading line by line.
        BufferedReader bIn = new BufferedReader(fIn);

        /*
         * The pipe should contain three lines: The note's title, an empty line, and the note's
         * contents. The following code reads and stores these three lines.
         */
        for (int index = 0; index < inputData.length; index++) {
            try {
                inputData[index] = bIn.readLine();
            } catch (IOException e) {

                e.printStackTrace();
                fail();
            }
        }

        // Asserts that the first record in the provider (written from TEST_NOTES[0]) has the same
        // note title as the first line retrieved from the pipe.
        assertEquals(TEST_NOTES[0].title, inputData[0]);

        // Asserts that the first record in the provider (written from TEST_NOTES[0]) has the same
        // note contents as the third line retrieved from the pipe.
        assertEquals(TEST_NOTES[0].note, inputData[2]);
    }
        ```
