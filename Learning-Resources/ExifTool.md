# ExifTool Fundamentals
### *Write-Up Update June 30 2026*

#### - Method 1: ExifTool Manual Redirect (Recommended)
- First, you can manually navigate to the directory where your file is in.  For example, you can start by typing in:

<sub>Mac/Linux zsh</sub>
```bash title="Mac/Linux zsh"
cd /path/to/file/folder
```
<sub>Windows CMD</sub>

```cmd
cd (DISK):\path\to\file\folder
```
- Once the terminal redirects to that directory where you have your file on, you can simply type

<sub>All Platforms</sub>
```bash
exiftool FILENAME.EXTENSION
```

***Tip:** Change path/to/file/folder to the actual path of the file, and change FILENAME.EXTENSION to the file you have.*

#### - Method 2: ExifTool Drag and Drop (Faster!)
- The method I used to run ExifTool in *bash/zsh (Mac and Linux)*, is to open up the terminal app, and type in

<sub>Mac/Linux zsh</sub>
```bash
exiftool #with a whitespace!
```
- But don't press enter yet. You will drag and drop the file into the terminal window. This will automatically input the full path for you. the final input should look something like this:

<sub>Mac/Linux zsh</sub>
```bash
exiftool /path/to/file/folder/FILENAME.EXTENSION
```


#### - Method 3: Online Tools
- Websites like **[Jimpl](https://jimpl.com/)**, **[EXIF.tools](https://exif.tools/)**, or **[METADATA2GO](https://www.metadata2go.com/view-metadata)** can extract the file's metadata by just dragging and dropping into the entire webpage or the space provided.
