This is a Contact Management System built using Python and Tkinter, with SQLite as the database backend. It provides a graphical user interface (GUI) to manage contact details, allowing users to add, search, update, delete, and display contact records.

📌 Key Features of the System
Graphical User Interface (GUI)

Built using Tkinter.
Various widgets like Entry, Button, Label, Treeview, OptionMenu, and Scrollbar are used.
Database Integration

Uses SQLite for storing contact details.
A table named REGISTRATION is created to store data.
CRUD Operations (Create, Read, Update, Delete)

Add New Contact: Users can enter and save new contact details.
Search Contact: Users can search by first name.
Update Contact: Allows modification of existing records.
Delete Contact: Removes a selected contact from the database.
View All Contacts: Displays all stored records in a table.
Data Validation

Prevents adding empty records.
Confirms deletions before executing.
📂 Code Breakdown
1️⃣ Database Setup
python
Copy
Edit
def Database():
    global conn, cursor
    conn = sqlite3.connect("contact.db")
    cursor = conn.cursor()
    cursor.execute(
        "CREATE TABLE IF NOT EXISTS REGISTRATION (RID INTEGER PRIMARY KEY AUTOINCREMENT NOT NULL, FNAME TEXT, LNAME TEXT, GENDER TEXT, EMAIL TEXT NOT NULL, ADDRESS TEXT, CONTACT TEXT)")
Creates a database (contact.db) if it doesn’t exist.
Defines a table REGISTRATION with columns:
RID (Primary Key)
FNAME, LNAME (First & Last Name)
GENDER
EMAIL (Unique identifier)
ADDRESS
CONTACT (Phone number)
2️⃣ GUI Layout
python
Copy
Edit
def DisplayForm():
    display_screen = Tk()
    display_screen.geometry("900x400")
    display_screen.title("PYTHON MINI SKILL PROJECT BY Bhupendra Meena")
Creates the main window (Tk()).
Sets window size and title.
Sections of the GUI
Left Panel: Registration Form

Input fields for name, gender, email, address, and contact.
A Submit button to add contacts.
Middle Panel: Data Table (Treeview)

Displays stored contacts.
Supports double-clicking a row to populate form fields for updating.
Right Panel: Search & Operations

Search field to look up contacts by first name.
Buttons for Search, View All, Reset, Delete, and Update.
3️⃣ Registering a Contact
python
Copy
Edit
def register():
    Database()
    fname1=fname.get()
    lname1=lname.get()
    gender1=gender.get()
    email1=email.get()
    address1=address.get()
    contact1=contact.get()
    
    if fname1=='' or lname1=='' or gender1=='' or email1=='' or address1=='' or contact1=='':
        tkMessageBox.showinfo("Warning", "Fill the empty field!!!")
    else:
        conn.execute('INSERT INTO REGISTRATION (FNAME,LNAME,GENDER,EMAIL,ADDRESS,CONTACT) VALUES (?,?,?,?,?,?)',
                     (fname1, lname1, gender1, email1, address1, contact1))
        conn.commit()
        tkMessageBox.showinfo("Message", "Stored successfully")
        DisplayData()
        conn.close()
Retrieves user input.
Validates that no field is empty.
Inserts data into REGISTRATION table.
Displays a success message and refreshes the table.
4️⃣ Updating a Contact
python
Copy
Edit
def Update():
    Database()
    fname1=fname.get()
    lname1=lname.get()
    gender1=gender.get()
    email1=email.get()
    address1=address.get()
    contact1=contact.get()
    
    if fname1=='' or lname1=='' or gender1=='' or email1=='' or address1=='' or contact1=='':
        tkMessageBox.showinfo("Warning", "Fill the empty field!!!")
    else:
        curItem = tree.focus()
        contents = (tree.item(curItem))
        selecteditem = contents['values']
        
        conn.execute('UPDATE REGISTRATION SET FNAME=?,LNAME=?,GENDER=?,EMAIL=?,ADDRESS=?,CONTACT=? WHERE RID = ?',
                     (fname1, lname1, gender1, email1, address1, contact1, selecteditem[0]))
        conn.commit()
        tkMessageBox.showinfo("Message", "Updated successfully")
        Reset()
        DisplayData()
        conn.close()
Captures selected contact from the table.
Updates the record in the database.
5️⃣ Deleting a Contact
python
Copy
Edit
def Delete():
    Database()
    if not tree.selection():
        tkMessageBox.showwarning("Warning", "Select data to delete")
    else:
        result = tkMessageBox.askquestion('Confirm', 'Are you sure you want to delete this record?', icon="warning")
        if result == 'yes':
            curItem = tree.focus()
            contents = (tree.item(curItem))
            selecteditem = contents['values']
            tree.delete(curItem)
            cursor=conn.execute("DELETE FROM REGISTRATION WHERE RID = %d" % selecteditem[0])
            conn.commit()
            cursor.close()
            conn.close()
Checks if a contact is selected.
Prompts for confirmation before deletion.
Deletes the record from REGISTRATION.
6️⃣ Searching a Contact
python
Copy
Edit
def SearchRecord():
    Database()
    if SEARCH.get() != "":
        tree.delete(*tree.get_children())
        cursor=conn.execute("SELECT * FROM REGISTRATION WHERE FNAME LIKE ?", ('%' + str(SEARCH.get()) + '%',))
        fetch = cursor.fetchall()
        for data in fetch:
            tree.insert('', 'end', values=(data))
        cursor.close()
        conn.close()
Fetches contacts matching the entered first name.
Displays results in the table.
7️⃣ Displaying All Contacts
python
Copy
Edit
def DisplayData():
    Database()
    tree.delete(*tree.get_children())
    cursor=conn.execute("SELECT * FROM REGISTRATION")
    fetch = cursor.fetchall()
    for data in fetch:
        tree.insert('', 'end', values=(data))
        tree.bind("<Double-1>", OnDoubleClick)
    cursor.close()
    conn.close()
Clears current data.
Fetches and displays all records from the database.
8️⃣ Handling Row Clicks
python
Copy
Edit
def OnDoubleClick(self):
    curItem = tree.focus()
    contents = (tree.item(curItem))
    selecteditem = contents['values']
    fname.set(selecteditem[1])
    lname.set(selecteditem[2])
    gender.set(selecteditem[3])
    email.set(selecteditem[4])
    address.set(selecteditem[5])
    contact.set(selecteditem[6])
When a row is double-clicked, the data is loaded into input fields for editing.
🔥 Potential Improvements
✅ Better UI Styling
✅ Phone & Email Validation
✅ Export Contacts as CSV/Excel
✅ Login Authentication (for multi-user access)
✅ Search by More Fields (Email, Contact, Address)

🎯 Conclusion
This Contact Management System provides a basic but functional way to store and manage contacts using a GUI-based interface. It allows users to add, view, search, update, and delete contacts. With some improvements, it could be expanded into a more advanced address book or CRM system. 🚀
