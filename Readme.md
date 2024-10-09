#Complaint Management System Project in Python

a GUI is created using the Tkinter python (library acts as frontend for the project).

1. Main.py- 


1. **Libraries Imported**:
   - **Tkinter**: Used to create the Graphical User Interface (GUI) for the complaint management system.
   - **DBConnect**: Handles saving complaints to the database (from `db.py`).
   - **ListComp**: Displays the list of complaints (from `listComp.py`).

2. **Setting Up the Window (root)**:
   - A window is created using `Tk()`, setting the size (600x285), title ("Complaint Management"), and background color.

3. **Styling**:
   - A basic style is applied to all labels, buttons, and radio buttons to give them a consistent look.

4. **Labels and Buttons**:
   - Three labels: "Full Name", "Gender", and "Comment" are added to guide users for input.
   - Two buttons: "Submit Now" (to save data) and "List Comp." (to show all complaints).

5. **User Input Fields**:
   - **Full Name**: An input field (Entry) where the user types their name.
   - **Gender**: Two radio buttons to choose between "Male" and "Female."
   - **Comment**: A text area (Text) where the user can type their complaint.

6. **Functions**:
   - **SaveData()**: This function is called when the "Submit Now" button is pressed. It:
     1. Grabs the user's name, gender, and comment.
     2. Sends the data to the database via `conn.Add()` (from `DBConnect`).
     3. Clears the input fields after saving the complaint.
     4. Shows a message to the user indicating whether the data was added successfully.
   
   - **ShowList()**: This function is called when the "List Comp." button is pressed. It displays all the stored complaints via `ListComp()`.

7. **Button Commands**:
   - The `command` attribute is used to link the buttons with the functions: "Submit Now" is linked to `SaveData()`, and "List Comp." is linked to `ShowList()`.

8. **Event Loop**:
   - `root.mainloop()` keeps the window open and responsive to user actions until it is closed.

### How it Works:
1. User inputs their name, selects gender, writes a comment, and clicks "Submit Now."
2. The `SaveData()` function saves this data to the database and clears the input fields.
3. If the user clicks "List Comp.," the `ShowList()` function opens a new window to display all complaints.





2. db.y

### Key Components:

1. **`sqlite3`**: 
   - This is a Python library for interacting with SQLite databases. SQLite is a lightweight, file-based database.
   - No need to set up a complex server; it works with a local `.db` file.

2. **`DBConnect` Class**:
   - A class is like a blueprint for creating objects. Here, it represents the database connection and operations related to it.

#### `__init__` Method:
- This method is automatically called when an object of the class is created. 
- **What it does**:
  - `sqlite3.connect('information.db')`: Opens a connection to the database file (`information.db`). If this file doesn’t exist, it will create one.
  - `row_factory`: This helps access database rows like dictionaries, so you can refer to column names.
  - **Table creation**: 
    - The line `self._db.execute('create table if not exists...')` ensures that a table named `Comp` is created if it doesn't already exist. 
    - This table stores complaints with four fields:
      - **ID**: A unique number for each complaint, generated automatically.
      - **Name**: The complainant's name.
      - **Gender**: Male or Female.
      - **Comment**: The actual complaint text.
  - `self._db.commit()`: Saves the changes made to the database, like creating the table.

#### `Add()` Method:
- This method is responsible for **inserting a new complaint** into the database.
- **Parameters**: `name`, `gender`, `comment` (all are provided when the user submits the form).
- **What it does**:
  - `self._db.execute('insert into Comp...')`: Inserts the provided values (name, gender, and comment) into the `Comp` table.
  - `self._db.commit()`: Saves this new entry into the database permanently.
  - The method then returns a message saying, “Your complaint has been submitted.”

#### `ListRequest()` Method:
- This method **fetches all complaints** from the `Comp` table.
- **What it does**:
  - `cursor = self._db.execute('select * from Comp')`: Selects all rows (complaints) from the `Comp` table.
  - `return cursor`: Returns the result of the query, which can be used to display the complaints in the GUI.

### Overall Logic:
- When the app starts, the `DBConnect` object initializes the database and ensures that the `Comp` table is ready.
- When a complaint is submitted, the `Add()` method is called to save the data.
- To view all complaints, `ListRequest()` is called, which retrieves and returns all the data from the `Comp` table.

### Summary:
- **Database Setup**: `__init__` creates the table and connects to the database.
- **Save Data**: `Add()` inserts a new complaint into the database.
- **Retrieve Data**: `ListRequest()` fetches all complaints for viewing.





3. listComp.py

### Key Components:

1. **Libraries Imported**:
   - **Tkinter**: Used to create the GUI to display the list of complaints.
   - **DBConnect**: Imported from `db.py`, responsible for connecting to the database and fetching complaint data.
   - **sqlite3**: Allows interacting with SQLite databases.

### `ListComp` Class:
This class creates a new window that displays all the complaints.

#### `__init__` Method:
- **Database Connection**:
   - `self._dbconnect = DBConnect()`: Creates an object of the `DBConnect` class, which connects to the database (`information.db`).
   - `self._dbconnect.row_factory = sqlite3.Row`: Allows rows fetched from the database to be accessed like dictionaries, meaning you can refer to the column names (e.g., `row['Name']`).

- **GUI Setup**:
   - `self._root = Tk()`: Creates a new window with the title "List of Complaints."
   - A `Treeview` widget (like a table) is created and packed into the window using `tv = Treeview(self._root)`.
   - The columns (`ID`, `Name`, `Gender`, and `Comment`) are set up with `tv.heading()` to define headers and `tv.configure()` to define columns.

- **Fetching and Displaying Data**:
   - `cursor = self._dbconnect.ListRequest()`: Fetches all the complaints from the database using the `ListRequest()` method from `db.py`.
   - The `for` loop goes through each row of complaints, inserting the data into the `Treeview` widget.
     - `tv.insert()`: Adds a new row for each complaint, showing the `ID`.
     - `tv.set()`: Sets the values of `Name`, `Gender`, and `Comment` in the respective columns for each row.

### Overall Logic:
- The class fetches complaint data from the database and displays it in a neatly organized table using Tkinter's `Treeview` widget. Each complaint’s ID, name, gender, and comment are shown in separate columns.

### Flow of Actions:
1. When a user wants to view complaints, this window opens.
2. The complaints are fetched from the database and displayed in a table format.
3. Each row in the table shows a complaint with the associated details (ID, name, gender, and comment).
















































