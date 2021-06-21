# Overview

This is a modified version of minwiki2 that's designed to store files on OneDrive. 

Keeping files in sync is an exercise in dealing with tradeoffs. This project "solves" the syncing problem by taking advantage of the traditional wiki editing approach where the new version of a page isn't saved on the server until you've finished making all your edits. Specifically:

- You run a local webserver to view pages in the wiki.
- When you want to **view a page**, rclone pulls the latest version from OneDrive. 
  - The first case is where the latest version of the file already exists on the local computer. rclone doesn't download anything and the page is rendered in the browser. It might seem that this adds overhead to the viewing of wiki pages. In practice, it's almost always faster than viewing a static html page served by a website. Rather than downloading the full page, you only do a check to confirm that you have the latest version of the file. In other words, all of your pages are cached on the local computer, so the process is very fast.
  - The second case is where there's a newer version of the page on OneDrive. The file is downloaded to the local machine and then rendered in the browser. This is about the same speed as a static html page served by a website. 
  - The final case is where a page doesn't exist locally or in OneDrive. minwiki2 will try to pull the version from OneDrive. When that fails, it creates a new file on the local machine and opens it for editing.
- **Editing of files** is done on the local machine. Files are pushed to OneDrive using rclone after they're edited. Local editing is the only reasonable choice (for performance reasons) if you want to frequently save. Since the traditional approach for wikis is to only save the updated page on the server when you've finished editing, that is the only time the file is synced to OneDrive. You'll have to sync manually if you want to make changes outside of your wiki. This is *strongly discouraged* since there's no guarantee you will be editing the latest version of a file if you open it directly in an editor.
- If you use the wiki as intended, you are guaranteed to always:
  - Have all files in sync no matter how many machines you use or how many people are editing the wiki. 
  - Be viewing the latest version of every file.
  - Never forget to pull before editing (as with Git) or have a failed sync (if you close your laptop before an autosync has completed).

It's also worth noting that OneDrive will render markdown files online. That allows you to view your files any place you can access OneDrive.

# Limitations

Tag search and backlinks are not supported in this version of minwiki2. Doing that would require modifications that I've not yet made. There would need to be an "initialization" command that pulls all files to the local machine on setup plus a way to update every markdown file prior to every tag search or calculation of backlinks. While that's certainly straightforward to implement, the performance on a wiki of any size would mean you'd only use those features when you really need them.

An alternative would be to use rclone to mount your OneDrive wiki directory in a different directory on your local computer and use that directory for tag search and backlink operations. Given the completely different design of such a system, it is really a new project.

I haven't really had the urge to work on this problem, since I use this for taking quick notes that I want to be available everywhere. Maybe someday if I'm in the mood.

# Use With Version Control

One of the downsides of OneDrive is that it's not a real version control system. Since all your files are in markdown format, you can rather easily dump them in a repo. Be forewarned that you'd need to constantly be pulling and pushing as with any repo if you want to go that route. Merge conflicts are likely to be common. Ultimately, if you want to use something like Git, there's probably no reason to use this system. OneDrive really doesn't bring anything new to the table if you're not using it to simplify the syncing problem.
