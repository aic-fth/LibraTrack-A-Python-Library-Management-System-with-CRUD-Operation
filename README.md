# LibraTrack-A-Python-Library-Management-System-with-CRUD-Operation
LibraTrack is a lightweight Python-based Library Management System featuring a dedicated librarian interface. It offers CRUD operations to efficiently manage and track library inventory, making it ideal for small libraries and academic projects.


________________________________________________________________________________________


![image alt](https://github.com/aic-fth/LibraTrack-A-Python-Library-Management-System-with-CRUD-Operation/blob/main/Library%20Image.png?raw=true)


________________________________________________________________________________________


[Link Text](https://github.com/aic-fth/LibraTrack-A-Python-Library-Management-System-with-CRUD-Operation/blob/f1a037ae4f61f5e93987fbb3eaeb3c3f98258b18/LMS-PSEUDOCODE.txt)


________________________________________________________________________________________


**Pseudocode**

FUNCTION add_book()
    title ← GET entry_title input and strip whitespace
    author ← GET entry_author input and strip whitespace
    genre ← GET entry_genre input and strip whitespace

    IF title = "" OR author = "" OR genre = "" THEN
        SHOW warning message "Please fill all fields!"
    ELSE
        book_id ← "BK-" + format(len(books) + 1, "04d")
        books[book_id] ← (title, author, genre)
        CALL refresh_table()
        CALL clear_entries()
        SHOW info message "Book added with ID " + book_id
    END IF
END FUNCTION

FUNCTION delete_book()
    selected ← GET selected item from book_table
    IF selected is not empty THEN
        book_id ← GET values[1] from selected item
        IF book_id exists in books THEN
            DELETE books[book_id]
            CALL refresh_table()
            SHOW info message "Book " + book_id + " deleted successfully."
        END IF
    ELSE
        SHOW warning message "Please select a book to delete."
    END IF
END FUNCTION

FUNCTION update_book()
    search_id ← GET entry_search input and strip whitespace
    IF search_id = "" THEN
        SHOW warning message "Please enter a Book ID to search for updating!"
        RETURN
    END IF

    IF search_id exists in books THEN
        title ← GET entry_title input and strip whitespace
        author ← GET entry_author input and strip whitespace
        genre ← GET entry_genre input and strip whitespace

        IF title = "" OR author = "" OR genre = "" THEN
            SHOW warning message "Please fill all fields to update the book!"
            RETURN
        END IF

        books[search_id] ← (title, author, genre)
        CALL refresh_table()
        CALL clear_entries()
        SHOW info message "Book " + search_id + " updated successfully."
    ELSE
        SHOW info message "Book ID not found."
    END IF
END FUNCTION

FUNCTION remove_book()
    search_id ← GET entry_search input and strip whitespace
    IF search_id = "" THEN
        SHOW warning message "Please enter a Book ID to search for removal!"
        RETURN
    END IF

    IF search_id exists in books THEN
        DELETE books[search_id]
        CALL refresh_table()
        CALL clear_entries()
        SHOW info message "Book " + search_id + " removed successfully."
    ELSE
        SHOW info message "Book ID not found."
    END IF
END FUNCTION

FUNCTION search_book()
    search_id ← GET entry_search input and strip whitespace
    IF search_id = "" THEN
        SHOW warning message "Please enter a Book ID to search!"
        RETURN
    END IF

    IF search_id exists in books THEN
        (title, author, genre) ← books[search_id]
        CLEAR entry_title, entry_author, entry_genre
        SET entry_title to title
        SET entry_author to author
        SET entry_genre to genre
        CALL highlight_row(search_id)
    ELSE
        SHOW info message "Book ID not found."
    END IF
END FUNCTION

FUNCTION refresh_table()
    CLEAR all rows in book_table
    FOR EACH (book_id, (title, author, genre)) in books DO
        INSERT (title, book_id, author, genre) into book_table
    END FOR
END FUNCTION

FUNCTION clear_entries()
    CLEAR entry_title, entry_author, entry_genre, entry_search
END FUNCTION

FUNCTION highlight_row(book_id)
    FOR EACH row in book_table DO
        IF row values[1] = book_id THEN
            SELECT row
            FOCUS row
            SCROLL to row
            BREAK
        END IF
    END FOR
END FUNCTION

FUNCTION exit_app()
    CLOSE the window
END FUNCTION

MAIN PROGRAM
    CREATE main window with title "Library Management System"
    SET size to 700x600
    SET background color to "#D8B897"

    INITIALIZE books as empty dictionary

    DISPLAY header label

    CREATE search section with entry_search and Search button

    CREATE form section with fields: entry_title, entry_author, entry_genre

    CREATE button section with: Add Book, Update Book, Remove Book, Exit

    CREATE book_table with columns: Title, ID, Author, Genre
    APPLY styling to Treeview

    DISPLAY book_table

    START main event loop
END PROGRAM

_______________________________________________________________________________________________________________________________________________________________________________________________


**LMS CODE**

import tkinter as tk
from tkinter import ttk, messagebox


def add_book():
    title = entry_title.get().strip()
    author = entry_author.get().strip()
    genre = entry_genre.get().strip()

    if title == "" or author == "" or genre == "":
        messagebox.showwarning("Warning", "Please fill all fields!")
    else:
        book_id = f"BK-{len(books) + 1:04d}"
        books[book_id] = (title, author, genre)
        refresh_table()
        clear_entries()
        messagebox.showinfo("Success", f"Book added with ID {book_id}")

def delete_book():
    selected = book_table.selection()
    if selected:
        book_id = book_table.item(selected[0])['values'][1]
        if book_id in books:
            del books[book_id]
            refresh_table()
            messagebox.showinfo("Deleted", f"Book {book_id} deleted successfully.")
    else:
        messagebox.showwarning("Warning", "Please select a book to delete.")

def update_book():
    search_id = entry_search.get().strip()
    if search_id == "":
        messagebox.showwarning("Warning", "Please enter a Book ID to search for updating!")
        return

    if search_id in books:
        title = entry_title.get().strip()
        author = entry_author.get().strip()
        genre = entry_genre.get().strip()

        if title == "" or author == "" or genre == "":
            messagebox.showwarning("Warning", "Please fill all fields to update the book!")
            return
        
        books[search_id] = (title, author, genre)
        refresh_table()
        clear_entries()
        messagebox.showinfo("Updated", f"Book {search_id} updated successfully.")
    else:
        messagebox.showinfo("Not Found", "Book ID not found.")

def remove_book():
    search_id = entry_search.get().strip()
    if search_id == "":
        messagebox.showwarning("Warning", "Please enter a Book ID to search for removal!")
        return

    if search_id in books:
        del books[search_id]
        refresh_table()
        clear_entries()
        messagebox.showinfo("Removed", f"Book {search_id} removed successfully.")
    else:
        messagebox.showinfo("Not Found", "Book ID not found.")

def search_book():
    search_id = entry_search.get().strip()
    if search_id == "":
        messagebox.showwarning("Warning", "Please enter a Book ID to search!")
        return

    if search_id in books:
        title, author, genre = books[search_id]
        entry_title.delete(0, tk.END)
        entry_author.delete(0, tk.END)
        entry_genre.delete(0, tk.END)

        entry_title.insert(0, title)
        entry_author.insert(0, author)
        entry_genre.insert(0, genre)

        highlight_row(search_id)
    else:
        messagebox.showinfo("Not Found", "Book ID not found.")

def refresh_table():
    book_table.delete(*book_table.get_children())
    for book_id, (title, author, genre) in books.items():
        book_table.insert("", tk.END, values=(title, book_id, author, genre))

def clear_entries():
    entry_title.delete(0, tk.END)
    entry_author.delete(0, tk.END)
    entry_genre.delete(0, tk.END)
    entry_search.delete(0, tk.END)

def highlight_row(book_id):
    for item in book_table.get_children():
        if book_table.item(item, "values")[1] == book_id:
            book_table.selection_set(item)
            book_table.focus(item)
            book_table.see(item)
            break

def exit_app():
    window.destroy()


window = tk.Tk()
window.title("Library Management System")
window.geometry("700x600")
window.configure(bg="#D8B897")

books = {}

tk.Label(window, text="Library Management System", font=("Arial", 24, "bold"), bg="#8D6E63", fg="white").pack(fill=tk.X, pady=10)


frame_search = tk.Frame(window, bg="#D8B897")
frame_search.pack(pady=5)

tk.Label(frame_search, text="Search Book ID:", bg="#D8B897", font=("Arial", 12)).pack(side=tk.LEFT, padx=5)
entry_search = tk.Entry(frame_search, width=20, font=("Arial", 12))
entry_search.pack(side=tk.LEFT, padx=5)
tk.Button(frame_search, text="Search", command=search_book, bg="#A9746E", fg="white", font=("Arial", 12)).pack(side=tk.LEFT, padx=5)


frame_form = tk.Frame(window, bg="#D8B897")
frame_form.pack(pady=10)

tk.Label(frame_form, text="Book Title:", bg="#D8B897", font=("Arial", 12)).grid(row=0, column=0, pady=5, sticky="w")
entry_title = tk.Entry(frame_form, width=30, font=("Arial", 12))
entry_title.grid(row=0, column=1, pady=5, padx=10)

tk.Label(frame_form, text="Author Name:", bg="#D8B897", font=("Arial", 12)).grid(row=1, column=0, pady=5, sticky="w")
entry_author = tk.Entry(frame_form, width=30, font=("Arial", 12))
entry_author.grid(row=1, column=1, pady=5, padx=10)

tk.Label(frame_form, text="Genre:", bg="#D8B897", font=("Arial", 12)).grid(row=2, column=0, pady=5, sticky="w")
entry_genre = tk.Entry(frame_form, width=30, font=("Arial", 12))
entry_genre.grid(row=2, column=1, pady=5, padx=10)


frame_buttons = tk.Frame(window, bg="#D8B897")
frame_buttons.pack(pady=10)

tk.Button(frame_buttons, text="Add Book", width=12, command=add_book, bg="#A9746E", fg="white", font=("Arial", 12)).grid(row=0, column=0, padx=5)
tk.Button(frame_buttons, text="Update Book", width=12, command=update_book, bg="#A9746E", fg="white", font=("Arial", 12)).grid(row=0, column=1, padx=5)
tk.Button(frame_buttons, text="Remove Book", width=12, command=remove_book, bg="#A9746E", fg="white", font=("Arial", 12)).grid(row=0, column=2, padx=5)
tk.Button(frame_buttons, text="Exit", width=12, command=exit_app, bg="#A9746E", fg="white", font=("Arial", 12)).grid(row=0, column=4, padx=5)


columns = ("Title", "ID", "Author", "Genre")
book_table = ttk.Treeview(window, columns=columns, show="headings", style="Treeview.Heading")
for col in columns:
    book_table.heading(col, text=col, anchor="center")
    book_table.column(col, anchor="center", width=150)


style = ttk.Style()
style.configure("Treeview.Heading", font=("Arial", 10))  
style.configure("Treeview", font=("Arial", 10))  

book_table.pack(pady=10, fill=tk.BOTH, expand=True)

window.mainloop()
