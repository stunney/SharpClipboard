# SharpClipboard
[![sc-nuget](/Assets/nuget-package-2.0.5-brightgreen.svg)](https://www.nuget.org/packages/SharpClipboard/) [![sc-donate](/Assets/Donate-PayPal-blue.svg)](https://www.paypal.com/cgi-bin/webscr?cmd=_s-xclick&hosted_button_id=DJ8D9CE8BWA3J&source=url)

**SharpClipboard** is a clipboard-monitoring library for .NET that listens to the system's clipboard entries,
allowing developers to tap into the rich capabilities of determining the clipboard's contents at runtime.

Here's a screenshot and below a usage-preview of the library's features:

![sc-preview-01](/Assets/sharpclipboard-preview-01.png)
![sc-usage](/Assets/sharpclipboard-usage-01.gif)

# Installation
To install via the NuGet Package Manager Console, type:

> `Install-Package SharpClipboard -Version 2.0.3`

You can also [download](https://github.com/Willy-Kimura/SharpClipboard/releases/download/v2.0.1/SharpClipboard.dll) the assembly and add it to Visual Studio's Toolbox.

# Features
Here's a comprehensive list of the features available:

- Built as a component making it accessible in Design Mode.
- Silently monitors the system clipboard uninterrupted; can also be disabled while running.
- Ability to detect clipboard content in various formats, namely **text**, **images**, **files**, and **other complex types**.
- Option to control the type of content to be monitored, e.g. **text** only, **text** and **images** only.
- Ability to capture the background application's details from where the clipboard's contents were cut/copied. 
*For example:*
    ```c#
    private void ClipboardChanged(Object sender, SharpClipboard.ClipboardChangedEventArgs e)
    {
        // Gets the application's executable name.
        Debug.WriteLine(e.SourceApplication.Name);
        // Gets the application's window title.
        Debug.WriteLine(e.SourceApplication.Title);
        // Gets the application's process ID.
        Debug.WriteLine(e.SourceApplication.ID.ToString());
        // Gets the application's executable path.
        Debug.WriteLine(e.SourceApplication.Path);
    }
    ```
# Usage
If you prefer working with the Designer, simply add the library to Visual Studio's Toolbox and use the
*Properties* window to change its options:

![sc-preview-02](/Assets/sharpclipboard-preview-02.png)

To use it in code, first import `WK.Libraries.SharpClipboardNS` - the code below will then assist you: 
```c#
    var clipboard = new SharpClipboard();

    // Attach your code to the ClipboardChanged event to listen to cuts/copies.
    clipboard.ClipboardChanged += ClipboardChanged;
    
    private void ClipboardChanged(Object sender, ClipboardChangedEventArgs e)
    {
        // Is the content copied of text type?
        if (e.ContentType == SharpClipboard.ContentTypes.Text)
        {
            // Get the cut/copied text.
            Debug.WriteLine(clipboard.ClipboardText);
        }

        // Is the content copied of image type?
        else if (e.ContentType == SharpClipboard.ContentTypes.Image)
        {
            // Get the cut/copied image.
            Image img = clipboard.ClipboardImage;
        }

        // Is the content copied of file type?
        else if (e.ContentType == SharpClipboard.ContentTypes.Files)
        {
            // Get the cut/copied file/files.
            Debug.WriteLine(clipboard.ClipboardFiles.ToArray());

            // ...or use 'ClipboardFile' to get a single copied file.
            Debug.WriteLine(clipboard.ClipboardFile);
        }

        // If the cut/copied content is complex, use 'Other'.
        else if (e.ContentType == SharpClipboard.ContentTypes.Other)
        {
            // Do something with 'e.Content' here...
        }
    }
```

You can also get the details of the application from where the clipboard's contents were cut/copied from using the `ClipboardChanged` argument property `SourceApplication`:

```c#
    private void ClipboardChanged(Object sender, SharpClipboard.ClipboardChangedEventArgs e)
    {
        // Gets the application's executable name.
        Debug.WriteLine(e.SourceApplication.Name);
        // Gets the application's window title.
        Debug.WriteLine(e.SourceApplication.Title);
        // Gets the application's process ID.
        Debug.WriteLine(e.SourceApplication.ID.ToString());
        // Gets the application's executable path.
        Debug.WriteLine(e.SourceApplication.Path);
    }
```

This option could come in handy especially when you're building a clipboard-monitoring application where users may feel the need to know where every recorded cut/copy action occurred.

To manually parse the content after a cut/copy has been detected, you can use the  argument property `e.Content` in the `ClipboardChanged` event:

```c#
    private void ClipboardChanged(Object sender, ClipboardChangedEventArgs e)
    {
        // For texts...
        string text = e.Content.ToString();

        // or images...
        Image img = (Image)e.Content;

        // or files...
        List<string> files = (List<string>)e.Content;

        // or other complex types too.
    }
```

## Donate
Hey, you can always [buy me a coffee](https://www.paypal.com/cgi-bin/webscr?cmd=_s-xclick&hosted_button_id=DJ8D9CE8BWA3J&source=url) if this component library ([or others](https://github.com/Willy-Kimura/BetterFolderBrowser)) has been of value to you.