
# Ex.No:3 Create Your Own Content Providers to get Contacts details.


## AIM:

To create your own content providers to get contacts details using Android Studio.

## EQUIPMENTS REQUIRED:

Android Studio(Min. required Artic Fox)

## ALGORITHM:

Step 1: Open Android Stdio and then click on File -> New -> New project.

Step 2: Then type the Application name as “ex.no.3″ and click Next. 

Step 3: Then select the Minimum SDK as shown below and click Next.

Step 4: Then select the Empty Activity and click Next. Finally click Finish.

Step 5: Design layout in activity_main.xml.

Step 6: Get contacts details and Display details give in MainActivity file.

Step 7: Save and run the application.

## PROGRAM:
```
/*
Program to print the text create your own content providers to get contacts details.
Developed by:VAZHIGA BANU S
Registeration Number : 212225040484
*/
```
## MainActivity.Java 
```
package com.example.contentprovider;

import android.Manifest;
import android.content.pm.PackageManager;
import android.database.Cursor;
import android.net.Uri;
import android.os.Bundle;
import android.provider.ContactsContract;
import android.util.Log;
import android.widget.Button;
import android.widget.TextView;

import androidx.appcompat.app.AppCompatActivity;
import androidx.core.app.ActivityCompat;
import androidx.core.content.ContextCompat;

public class MainActivity extends AppCompatActivity {

    Button button;
    TextView contactResult;

    private static final int REQUEST_CODE = 101;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        button = findViewById(R.id.button);
        contactResult = findViewById(R.id.contactResult);

        button.setOnClickListener(v -> {
            Log.i("FRIEND_CONTACT_APP", "Load Contacts Button Clicked");
            checkPermission();
        });
    }

    private void checkPermission() {

        if (ContextCompat.checkSelfPermission(
                this, Manifest.permission.READ_CONTACTS)
                != PackageManager.PERMISSION_GRANTED) {

            ActivityCompat.requestPermissions(
                    this,
                    new String[]{Manifest.permission.READ_CONTACTS},
                    REQUEST_CODE
            );

        } else {
            loadContacts();
        }
    }

    private void loadContacts() {

        StringBuilder result = new StringBuilder();

        Uri uri = ContactsContract.CommonDataKinds.Phone.CONTENT_URI;

        try (Cursor cursor = getContentResolver().query(
                uri,
                null,
                null,
                null,
                ContactsContract.CommonDataKinds.Phone.DISPLAY_NAME + " ASC")) {

            if (cursor != null && cursor.moveToFirst()) {

                int count = cursor.getCount();

                result.append("TOTAL CONTACTS: ")
                        .append(count)
                        .append("\n\n");

                Log.i("FRIEND_CONTACT_APP",
                        "Total Contacts: " + count);

                do {

                    String name = cursor.getString(
                            cursor.getColumnIndexOrThrow(
                                    ContactsContract.CommonDataKinds.Phone.DISPLAY_NAME));

                    String number = cursor.getString(
                            cursor.getColumnIndexOrThrow(
                                    ContactsContract.CommonDataKinds.Phone.NUMBER));

                    result.append("👤 ")
                            .append(name)
                            .append("\n")
                            .append("☎ ")
                            .append(number)
                            .append("\n\n");

                    Log.i("FRIEND_CONTACT_APP",
                            "Name: " + name + " | Number: " + number);

                } while (cursor.moveToNext());

                contactResult.setText(result.toString());

            } else {
                contactResult.setText("No contacts available.");
            }

        } catch (Exception e) {
            Log.e("FRIEND_CONTACT_APP", "Contact error", e);
            contactResult.setText("Error: " + e.getMessage());
        }
    }

    @Override
    public void onRequestPermissionsResult(
            int requestCode,
            String[] permissions,
            int[] grantResults) {

        super.onRequestPermissionsResult(
                requestCode, permissions, grantResults);

        if (requestCode == REQUEST_CODE) {

            if (grantResults.length > 0 &&
                    grantResults[0] == PackageManager.PERMISSION_GRANTED) {

                loadContacts();

            } else {
                contactResult.setText("Please allow contact permission.");
            }
        }
    }
}
```
## Activity_main.xml
```
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:background="#111827">

    <TextView
        android:id="@+id/header"
        android:layout_width="0dp"
        android:layout_height="180dp"
        android:background="#C9A76EB6"
        android:gravity="center"
        android:text="MY CONTACTS"
        android:textColor="#FFFFFF"
        android:textSize="28sp"
        android:textStyle="bold"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintEnd_toEndOf="parent"/>

    <TextView
        android:id="@+id/subtitle"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Manage your phone contacts"
        android:textColor="#C4B5FD"
        android:textSize="14sp"
        app:layout_constraintTop_toTopOf="@id/header"
        app:layout_constraintBottom_toBottomOf="@id/header"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        android:layout_marginTop="55dp"/>

    <Button
        android:id="@+id/button"
        android:layout_width="0dp"
        android:layout_height="58dp"
        android:layout_marginStart="28dp"
        android:layout_marginEnd="28dp"
        android:layout_marginTop="28dp"
        android:backgroundTint="#CDB0D1"
        android:text="LOAD CONTACTS"
        android:textColor="#1F2937"
        android:textSize="17sp"
        android:textStyle="bold"
        app:layout_constraintTop_toBottomOf="@id/header"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintEnd_toEndOf="parent"/>

    <TextView
        android:id="@+id/contactResult"
        android:layout_width="0dp"
        android:layout_height="0dp"
        android:layout_margin="20dp"
        android:background="#1F2937"
        android:padding="18dp"
        android:gravity="top"
        android:text="Your contacts will appear here..."
        android:textColor="#EADDEB"
        android:textSize="15sp"
        android:scrollbars="vertical"
        app:layout_constraintTop_toBottomOf="@id/button"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintEnd_toEndOf="parent"/>

</androidx.constraintlayout.widget.ConstraintLayout>
```
## AndroidManifest.xml
```
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android">

    <uses-permission
        android:name="android.permission.READ_CONTACTS" />

    <application
        android:allowBackup="true"
        android:label="Content Provider"
        android:supportsRtl="true"
        android:theme="@style/Theme.Contentprovider">

        <activity
            android:name=".MainActivity"
            android:exported="true">`

            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>

        </activity>

    </application>

</manifest>
```

## OUTPUT

<img width="1917" height="1198" alt="Screenshot 2026-11-04 090543" src="https://github.com/user-attachments/assets/116a1438-5cb7-4a79-a6db-a0823b9df071" />

<img width="1917" height="1198" alt="Screenshot 2026-11-04 093729" src="https://github.com/user-attachments/assets/4fb87f2e-e207-4b89-acc1-5133b32037bb" />

<img width="1687" height="943" alt="Screenshot 2026-11-06 082050" src="https://github.com/user-attachments/assets/f6e95fec-e29a-41cd-8556-01abbb9978aa" />


## RESULT
Thus a Simple Android Application create your own content providers to get contacts details using Android Studio is developed and executed successfully.
