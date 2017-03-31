---
layout: post
title:  "PhotoRenamer Part I"
date:   2017-03-13 15:02:43
tags: Java, JComponent, PhotoRenamer
---

Another Java project.

As a senior executive for a non-profit organization CACTES Association, I have to build and update its official website, including organizing photos and documents that are to be posted on the website. To keep the photos organized, I add information, such as the name of the event, as tags to each photo. Yet, sometimes I had to add tags to hundreds of photos... This was when I came up with the idea of the PhotoRenamer app.

The PhotoRenamer app allows users to add tags to photos in the selected directory, so it is easier for the user to search up tagged photos in the built-in operating system search.


First of all, I needed to build the back-end code. What would be the best data structure for a program that searches through files within files? The answer was clear- a Tree data structure. I created an abstract class called FileNode that takes in a file and stores its path and this specific file as attributes.

{% highlight ruby %}
public abstract class FileNode {
    private String name;
    private File file;
    public final static String atChar = "@";
    
    public FileNode(File f) {
        this.name = f.getAbsolutePath();
        this.file = f;
    }
}
{% endhighlight %}

Since not every file is going to be an imagine file, I made FileNode an abstract class and created another class called ImageFileNode that extends from FileNode class.

{% highlight ruby %}
public class ImageFileNode extends FileNode {
    private ArrayList<String> usedTags;
    private String extension;
    public Map<String, String> allRecords;
    
    public ImageFileNode(File f) {
        super(f);
        this.usedTags = new ArrayList<String>();
        this.allRecords = new HashMap<String, String>();
        this.extendsion = "";
        int i = f.getName().lastIndexOf('.');
        if (i>0) {
            extension = "." + f.getName().substring(i+1);
        }
    }
}
{% endhighlight %}

The ImageFileNode class stores the file's tags and extension, so in the future I can choose to see the used tags.
There are also many methods within the class to perform actions such as adding tags, removing tags, and seeing history.

Then, I created a node manager FileNodeManager that stores all the images in a selected directory as ImageFileNode and performs actions by calling methods on each node.

Lastly, I created a class Renamer that initializes a FileNodeManager upon calling and appends all the image files in the choosen file.

Go to part2 to check out the GUI.

You can check out the code on my [github page].

[github page]: https://github.com/edward0414/PhotoRenamer
