---
layout: post
title:  "ContentCopier"
date:   2017-03-19 12:50:23
tags: C, ContentCopier, fork(), fread(), fwrite()
---

This is a program coded in C.

When you are copying the contents from a directory to another directory, don't you find it annoying to click through all the directories to reach the destinated directory? That is how I felt too! Therefore, I made this program in C using function calls I learned to complete the task in command line.


To complete copying files action, I utilized fwrite and fread function calls. I first opened both the destination and source files using fopen. Once they were opened, I read the contents from the source file one byte at a time and then write the contents to the newly created destinated file at the same time.

{% highlight ruby %}
int copy_file(const char *src, const char *dest) {
    FILE *fr, *fw;
    char buffer[BUFFSIZE];
    
    if((fr = fopen(src,"rb")) == NULL){
        printf("cannot open this file for reading: %s.\n", src);
        return -1;
    }
    if((fw = fopen(dest, "wb")) == NULL){
        printf("cannot open this file for writing: %s.\n", dest);
        return -1;
    }
    
    size_t bytes;
    while ((bytes = fread(buffer, 1, BUFFSIZE, fr)) != 0) {
        size_t written = fwrite(buffer, 1, bytes, fw);
        if( written != bytes) {
            printf("eroor in writing.\n");
            break;
        }
    }
    fclose(fr);
    fclose(fw);
    return 0;
}
{% endhighlight %}

Since copy_file is an expensive function, I tried to check the size and the hash of the source and destinated files to see if the source file was already existed in the destinated directory. 

{% highlight ruby %}
if (same_size(src_filename, dest_filename)) {
//same size
    FILE *fp = fopen(src_filename, "r");
    FILE *fp2 = fopen(dest_filename, "r");
    char *hash1 = hash(fp);
    char *hash2 = hash(fp2);
    fclose(fp);
    fclose(fp2);

    if (hash1 == hash2) {
    //check hash, same hash
        if ((update_permission(src_filename, dest_filename)) == -1){
            printf("chmod fails for %s.", dest_filename);
            exit(1);
        }
    } else {
        copy_file(src_filename, dest_filename);
        if ((update_permission(src_filename, dest_filename)) == -1){
            printf("chmod fails for %s.", dest_filename);
            exit(1);
        }
        free(src_filename);
        free(dest_filename);
}
{% endhighlight %}

Lastly, in the case when the program is copying directories inside the directory, I had to do recursion call to recursively check the contents inside the copying directory. To reduce the running time, I made the program parallel by creating child processes within each sub-directory that run at the same time. 

{% highlight ruby %}
if (dp->d_type == DT_DIR) {
//use fork
    int r;
    if ((r=fork() >0)) {
    //parent process
        if (!is_directory(dest_filename)) {
            mkdir(dest_filename, 0777);
        } else {
            if ((update_permission(src_filename, dest_filename)) == -1){
                printf("chmod fails");
                exit(1);
            }
        }
        int status;
        if (wait(&status) == -1) {
            perror("wait");
        }
                
        count += WEXITSTATUS(status);

    } else if (r==0) {
    //child process
        exit(copy_ftree(src_filename, dest_filename));

    } else {
        perror("fork");
        exit(1);
    }
}
{% endhighlight %}


Using this program on command line, I do not have to physically copy the contents of a directory and then click to another directory to paste anymore!


You can check out the complete code on my [github page].

[github page]: https://github.com/edward0414/ContentCopier
