//## ����NotePadӦ����������չ --ʱ������ѯ����
//NotePadԴ�룺 https://github.com/llfjfz/NotePad

//1.ʱ�������ʵ��
//        �ڲ����ļ�������ʱ���
//
//        �޸�ǰ��



<RelativeLayout android:layout_height="match_parent"
        android:layout_width="match_parent"
        xmlns:android="http://schemas.android.com/apk/res/android">
<TextView xmlns:android="http://schemas.android.com/apk/res/android"
        android:id="@android:id/text1"
        android:layout_width="match_parent"
        android:layout_height="?android:attr/listPreferredItemHeight"
        android:textAppearance="?android:attr/textAppearanceLarge"
        android:gravity="center_vertical"
        android:paddingLeft="5dip"
        android:singleLine="true"
        />
</RelativeLayout>



//�޸ĺ�����һ��TextView




<RelativeLayout android:layout_height="match_parent"
        android:layout_width="match_parent"
        xmlns:android="http://schemas.android.com/apk/res/android">
<TextView xmlns:android="http://schemas.android.com/apk/res/android"
        android:id="@android:id/text1"
        android:layout_width="match_parent"
        android:layout_height="?android:attr/listPreferredItemHeight"
        android:textAppearance="?android:attr/textAppearanceLarge"
        android:gravity="center_vertical"
        android:paddingLeft="5dip"
        android:singleLine="true"
        />
<TextView
        android:id="@+id/text2"
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:paddingLeft="5dip"
                android:singleLine="true"
                android:gravity="center_vertical"
                />
</RelativeLayout>

//        ��ϵͳĬ�ϵ�ʱ����ʾ��ʽ����Ϊ�����Զ���ʱ���ʽ�����ӿɶ���
//
//        �޸�ǰ��




public Uri insert(Uri uri, ContentValues initialValues) {
        if (sUriMatcher.match(uri) != NOTES) {
        throw new IllegalArgumentException("Unknown URI " + uri);
        }
        ContentValues values;
        if (initialValues != null) {
        values = new ContentValues(initialValues);

        } else {
        values = new ContentValues();
        }
        //ϵͳĬ����ʾ��ʱ��ʹ�ú��������б�ʾ�������Ķ�
        Long now = Long.valueOf(System.currentTimeMillis());
        Date date = new Date(now);
        //��ʱ���ʽ��Ϊ�����������ձ�ʾ��ʽ
        SimpleDateFormat simpleDateFormat = new SimpleDateFormat("yy-MM-dd HH:mm:ss");
        simpleDateFormat.setTimeZone(TimeZone.getTimeZone("GMT+08:00"));
        String dateFormat = simpleDateFormat.format(date);
        if (values.containsKey(NotePad.Notes.COLUMN_NAME_CREATE_DATE) == false) {
        values.put(NotePad.Notes.COLUMN_NAME_CREATE_DATE, dateFormat);
        }
        if (values.containsKey(NotePad.Notes.COLUMN_NAME_MODIFICATION_DATE) == false) {
        values.put(NotePad.Notes.COLUMN_NAME_MODIFICATION_DATE, dateFormat);
        }

        if (values.containsKey(NotePad.Notes.COLUMN_NAME_TITLE) == false) {
        Resources r = Resources.getSystem();
        values.put(NotePad.Notes.COLUMN_NAME_TITLE, r.getString(android.R.string.untitled));
        }
        if (values.containsKey(NotePad.Notes.COLUMN_NAME_NOTE) == false) {
        values.put(NotePad.Notes.COLUMN_NAME_NOTE, "");
        }
        SQLiteDatabase db = mOpenHelper.getWritableDatabase();
        long rowId = db.insert(
        NotePad.Notes.TABLE_NAME,
        NotePad.Notes.COLUMN_NAME_NOTE,
        values
        );

        if (rowId > 0) {

        Uri noteUri = ContentUris.withAppendedId(NotePad.Notes.CONTENT_ID_URI_BASE, rowId);
        getContext().getContentResolver().notifyChange(noteUri, null);
        return noteUri;
        }
        throw new SQLException("Failed to insert row into " + uri);
        }



        �޸ĺ�



public Uri insert(Uri uri, ContentValues initialValues) {
        if (sUriMatcher.match(uri) != NOTES) {
        throw new IllegalArgumentException("Unknown URI " + uri);
        }
        ContentValues values;
        if (initialValues != null) {
        values = new ContentValues(initialValues);

        } else {
        values = new ContentValues();
        }
        //ϵͳĬ����ʾ��ʱ��ʹ�ú��������б�ʾ�������Ķ�
        Long now = Long.valueOf(System.currentTimeMillis());
        Date date = new Date(now);
        //��ʱ���ʽ��Ϊ�����������ձ�ʾ��ʽ
        SimpleDateFormat simpleDateFormat = new SimpleDateFormat("yy-MM-dd HH:mm:ss");
        simpleDateFormat.setTimeZone(TimeZone.getTimeZone("GMT+08:00"));
        String dateFormat = simpleDateFormat.format(date);
        if (values.containsKey(NotePad.Notes.COLUMN_NAME_CREATE_DATE) == false) {
        values.put(NotePad.Notes.COLUMN_NAME_CREATE_DATE, dateFormat);
        }
        if (values.containsKey(NotePad.Notes.COLUMN_NAME_MODIFICATION_DATE) == false) {
        values.put(NotePad.Notes.COLUMN_NAME_MODIFICATION_DATE, dateFormat);
        }

        if (values.containsKey(NotePad.Notes.COLUMN_NAME_TITLE) == false) {
        Resources r = Resources.getSystem();
        values.put(NotePad.Notes.COLUMN_NAME_TITLE, r.getString(android.R.string.untitled));
        }
        if (values.containsKey(NotePad.Notes.COLUMN_NAME_NOTE) == false) {
        values.put(NotePad.Notes.COLUMN_NAME_NOTE, "");
        }
        SQLiteDatabase db = mOpenHelper.getWritableDatabase();
        long rowId = db.insert(
        NotePad.Notes.TABLE_NAME,
        NotePad.Notes.COLUMN_NAME_NOTE,
        values
        );

        if (rowId > 0) {

        Uri noteUri = ContentUris.withAppendedId(NotePad.Notes.CONTENT_ID_URI_BASE, rowId);
        getContext().getContentResolver().notifyChange(noteUri, null);
        return noteUri;
        }
        throw new SQLException("Failed to insert row into " + uri);
        }

//        ��updateNote()�����޸ģ�
//        �޸�ǰ��


private final void updateNote(String text, String title) {
        ContentValues values = new ContentValues();
        values.put(NotePad.Notes.COLUMN_NAME_MODIFICATION_DATE, System.currentTimeMillis());
        if (mState == STATE_INSERT) {
        if (title == null) {
        int length = text.length();
        title = text.substring(0, Math.min(30, length));
        if (length > 30) {
        int lastSpace = title.lastIndexOf(' ');
        if (lastSpace > 0) {
        title = title.substring(0, lastSpace);
        }
        }
        }
        values.put(NotePad.Notes.COLUMN_NAME_TITLE, title);
        } else if (title != null) {
        values.put(NotePad.Notes.COLUMN_NAME_TITLE, title);
        }
        values.put(NotePad.Notes.COLUMN_NAME_NOTE, text);
        getContentResolver().update(
        mUri,
        values,
        null,
        null
        );
        }

      //  �޸ĺ����Ĭ�ϵĺ�����ʱ���ʽ






private final void updateNote(String text, String title) {
        ContentValues values = new ContentValues();
        long now = System.currentTimeMillis();
        Date date = new Date(now);
        SimpleDateFormat simpleDateFormat = new SimpleDateFormat("yy-MM-dd HH:mm:ss");
        simpleDateFormat.setTimeZone(TimeZone.getTimeZone("GMT+08:00"));
        String dateFormat = simpleDateFormat.format(date);
        values.put(NotePad.Notes.COLUMN_NAME_MODIFICATION_DATE, dateFormat);
        if (mState == STATE_INSERT) {

        if (title == null) {
        int length = text.length();
        title = text.substring(0, Math.min(30, length));
        if (length > 30) {
        int lastSpace = title.lastIndexOf(' ');
        if (lastSpace > 0) {
        title = title.substring(0, lastSpace);
        }
        }
        }
        values.put(NotePad.Notes.COLUMN_NAME_TITLE, title);
        } else if (title != null) {
        values.put(NotePad.Notes.COLUMN_NAME_TITLE, title);
        }
        values.put(NotePad.Notes.COLUMN_NAME_NOTE, text);
        getContentResolver().update(
        mUri,
        values,
        null,
        null
        );
        }


//        ����ȡ����ʱ����и�ֵ����
//        �۲�Դ�����ǿ��Է��֣���SQLite���Ѿ������ʱ����������Ǿ�û�б�Ҫ�����޸ģ��������£�

public void onCreate(SQLiteDatabase db) {
        db.execSQL("CREATE TABLE " + NotePad.Notes.TABLE_NAME + " ("
        + NotePad.Notes._ID + " INTEGER PRIMARY KEY,"
        + NotePad.Notes.COLUMN_NAME_TITLE + " TEXT,"
        + NotePad.Notes.COLUMN_NAME_NOTE + " TEXT,"
        + NotePad.Notes.COLUMN_NAME_CREATE_DATE + " INTEGER,"
        + NotePad.Notes.COLUMN_NAME_MODIFICATION_DATE + " INTEGER"
        + ");");
        }

        ����ʱ�������ֵ��


        if (values.containsKey(NotePad.Notes.COLUMN_NAME_CREATE_DATE) == false) {
        values.put(NotePad.Notes.COLUMN_NAME_CREATE_DATE, dateFormat);
        }
        if (values.containsKey(NotePad.Notes.COLUMN_NAME_MODIFICATION_DATE) == false) {
        values.put(NotePad.Notes.COLUMN_NAME_MODIFICATION_DATE, dateFormat);
        }


//        �鿴NoteԴ����Է��֣���ֻ����ʾid�Լ�titile����û��ʱ�䣬���Ǽ���ʱ�����ʾ��cursor�ͻὫ������ʱ��һ��������
//        ����װ����Ӧ�����У�����һ�������޸Ĳ���ʾʱ�����

private static final String[] PROJECTION = new String[] {
        NotePad.Notes._ID, // 0
        NotePad.Notes.COLUMN_NAME_TITLE, // 1
        NotePad.Notes.COLUMN_NAME_MODIFICATION_DATE,//����
        };
private static final int COLUMN_INDEX_TITLE = 1;
@Override
protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setDefaultKeyMode(DEFAULT_KEYS_SHORTCUT);
        Intent intent = getIntent();
        if (intent.getData() == null) {
        intent.setData(NotePad.Notes.CONTENT_URI);
        }
        getListView().setOnCreateContextMenuListener(this);
        Cursor cursor = managedQuery(
        getIntent().getData(),
        PROJECTION,
        null,
        null,
        NotePad.Notes.DEFAULT_SORT_ORDER
        );
        String[] dataColumns = { NotePad.Notes.COLUMN_NAME_TITLE, NotePad.Notes.COLUMN_NAME_MODIFICATION_DATE } ;
        int[] viewIDs = { android.R.id.text1, R.id.text2 }
        SimpleCursorAdapter adapter
        = new SimpleCursorAdapter(
        this,
        R.layout.noteslist_item,
        cursor,
        dataColumns,
        viewIDs
        );
        setListAdapter(adapter);
        }

        String[] dataColumns = { NotePad.Notes.COLUMN_NAME_TITLE, NotePad.Notes.COLUMN_NAME_MODIFICATION_DATE };
        int[] viewIDs = { android.R.id.text1, R.id.text2 };