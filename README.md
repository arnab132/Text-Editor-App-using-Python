# Text-Editor-App-using-Python

Here, I will help you to build a simple Text Editor Application using Tkinter which is a very good beginner project for Tkinter.

Text Editor Application is an application where you can write your text, open any text file, you can edit any text file and you can also save a file if you want. In this tutorial, we will build a Text Editor Application from scratch.

Essential Elements for the Text editor application are as follows:

There is a Button widget called btn_open that is used for opening a file for editing

Second one is a Button widget called btn_save for saving a file

Third, there is a Text widget called txt_edit for creating and editing any text file.

The arrangement of three widgets is done in a way such that the two buttons are on the left-hand side of the window, and the text box is on the right-hand side. The Minimum height of the whole window should be 900 pixels and txt_edit should have a minimum width of 900 pixels. And The whole layout should be responsive if the window is resized, then txt_edit is resized as well. The width of the Frame that holds the buttons should not change.

The desired layout of the Text Editor Application can be achieved using the .grid() geometry manager. And this layout contains a single row and two columns:

On the left side, there is A narrow column for the buttons

On the right side, there is A wider column for the text box

In order to set the minimum sizes for the window and txt_edit, you just need to set the minsize parameters of the window methods .rowconfigure() and .columnconfigure() to 900. In order to handle the resizing, the weight parameters of these methods will be set to 1.

If you want both the buttons in the same column then you’ll need to create a Frame widget called fr_buttons. According to the above-shown sketch, the two buttons should be stacked vertically inside of this frame, having btn_open on top. This can be done either by .grid() or .pack() geometry manager. For now, you’ll just need to stick with .grid() as it is easier to work with it.


Output

![image](https://user-images.githubusercontent.com/22562694/120153189-0be6c000-c20c-11eb-879c-755bcc0781a8.png)

As you can see in the output, we have a basic text editor application in which we can write something and then save the text in a new file or use the Open button to open a file in the editor and then edit it.


1. Creating all the needed widgets

The code snippet that is used is as follows:

import tkinter as tk

window = tk.Tk()

window.title("Text Editor Application")

window.rowconfigure(0, minsize=900, weight=1)

window.columnconfigure(1, minsize=900, weight=1)

txt_edit = tk.Text(window)

fr_buttons = tk.Frame(window)

btn_open = tk.Button(fr_buttons, text="Open")

btn_save = tk.Button(fr_buttons, text="Save As...")

Explanation of the above code:

The first command is used to import the tkinter.

Then the next two lines are used to create a new window with the title "Text Editor Application".

The next two lines of code are used to set the row and column configurations.

Then the lines from 9 to 12 will create the four widgets you’ll need for the text box, the frame, and the open and save buttons.

window.rowconfigure(0, minsize=900, weight=1)

The above-given line in the code indicates The minsize parameter of .rowconfigure() is set to 900 and weight is set to 1.The first argument is 0, which is used to set the height of the first row to 900 pixels and makes sure that the height of the row grows proportionally to the height of the window. There’s only one row in the application layout, so these settings are applied to the entire window.

Then take a look at this line in the code :

window.columnconfigure(1, minsize=900, weight=1)

In the above code the .columnconfigure() to set the width and weight attributes of the column with index 1 to 900 and 1, respectively. Keep it in mind that, row and column indices are zero-based, so these settings apply only to the second column. By configuring the second column, the text box will expand and contract naturally when the window is resized, while the column containing the buttons will always remain at a fixed width.

2.Creation of Application Layout

btn_open.grid(row=0, column=0, sticky="ew", padx=5, pady=5)

btn_save.grid(row=1, column=0, sticky="ew", padx=5)

The above two lines of code will create a grid with two rows and one column in the fr_buttons frame since both btn_open and btn_save have their master attribute set to fr_buttons. btn_open is put in the first row and btn_save will be in the second row so that btn_open appears above btn_save in the layout, as planned in the above sketch.

The btn_open and btn_save both have their sticky attributes set to "ew", which forces the buttons to expand horizontally in both directions and in order to fill the entire frame. It makes sure both buttons are of the same size.

You place 5 pixels of padding around each button just by setting the padx and pady parameters to 5. The btn_open has vertical padding. Because it’s on top, the vertical padding offsets the button down from the top of the window a bit and makes sure that there’s a small gap between this and btn_save.

Now the fr_buttons is laid out and ready to go, you can just set up the grid layout for the rest of the window now:

fr_buttons.grid(row=0, column=0, sticky="ns")

txt_edit.grid(row=0, column=1, sticky="nsew")

These above two lines of code are used to create a grid with one row and two columns for window. You place fr_buttons in the first column and txt_edit in the second column so that fr_buttons appears to the left of txt_edit in the window layout.

The sticky parameter for fr_buttons will be set to "ns", which forces the whole frame to expand vertically and fill the entire height of its column. txt_edit is used to fill its entire grid cell because you set its sticky parameter to "nsew", which forces it to expand in every direction.

Now we have just created the buttons but these do not work until we add functioning to them So let's start adding the Functioning of Buttons:

1. Function to Open the File
The code snippet for open_file is as follows:


def open_file():
    """Open a file for editing."""
    filepath = askopenfilename(
        filetypes=[("Text Files", "*.txt"), ("All Files", "*.*")]
    )
    if not filepath:
        return
    txt_edit.delete(1.0, tk.END)
    with open(filepath, "r") as input_file:
        text = input_file.read()
        txt_edit.insert(tk.END, text)
    window.title(f"Text Editor Application - {filepath}")

Explanation:

The Lines from 3 to 5 use the askopenfilename dialog from the tkinter.filedialog module to display a file open dialog and store the selected file path to filepath.

Lines 6 and 7 checks to see if the user closes the dialog box or clicks the Cancel button. If so, then filepath will be None, and the function will return without executing any of the code to read the file and set the text of txt_edit.

Line 8 clears the current contents of txt_edit using .delete().

Lines 9 and 10 are used to open the selected file and .read() its contents before storing the text as a string.

Line number 11 assigns the string text to txt_edit using .insert().

Line 12 sets the title of the window so that it contains the path of the open file.

2. Function to Save the File
The code snippet for save_file is as follows:

def save_file():
    """Save the current file as a new file."""
    filepath = asksaveasfilename(
        defaultextension="txt",
        filetypes=[("Text Files", "*.txt"), ("All Files", "*.*")],
    )
    if not filepath:
        return
    with open(filepath, "w") as output_file:
        text = txt_edit.get(1.0, tk.END)
        output_file.write(text)
    window.title(f"Text Editor Application - {filepath}")


Explanation:

Lines 3 to 6 use the asksaveasfilename dialog box to get the desired save location from the user. The selected file path is stored in the filepath variable.

Lines 7 and 8 checks to see if the user closes the dialog box or clicks the Cancel button. If so, then filepath will be None, and the function will return without executing any of the code to save the text to a file.

Line 9 creates a new file at the selected file path.

Line 10 extracts the text from txt_edit with .get() method and assigns it to the variable text.

Line 11 writes text to the output file.

Line 12 updates the title of the window so that the new file path is displayed in the window title.
