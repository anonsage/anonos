AnonOS's file system is mainly defined in a database structure, though files can still be found in an 'almost flat' hierarchy.


- Contains
  - `File`: Pointer to location in disk/memory; each file is a symbolic link, when they are all gone, then it could be 'garbage collected'.
    - `Permission`: read-write-execute for owner-group-public.
    - Other common attributes: name, type, size, time-modified, time-created, hash..
    - Other group attributes: view/gui-type, num-file, num-group..
  - `FileGroup` (aka directory, folder, namespace): Collection of groups and files.
    - Implementation detail is mainly important for high number of items. Main tasks involved would be add/remove/find/list. For best 'find' speed use hash table, for best 'space' use sorted tree. For 'list' task, need to keep a sorted order, so time can be spread out over each add/remove or it can all be done at 'list' time. The best would depend on which is done more, though if equal, user/GUI would have less lag when sorting is done at add/remove time with the sorted tree implementation, which should likely be default unless user knows what they are doing (v1 less options, of course).

Ex:

    /
        .config ;; Location of different important top-level groups, like os/, and user/. They could be on different mounts or virtual locations. Also, specify which user is owner.
        os/
            .config ;; Point to other location(s) of these default groups.
            app/ ;; Things that can initiate code to run (and their individual code/data).
            driver/ ;; Maybe a library?
            kernel/ ;; Maybe os/ sibling instead?
            lib/ ;; Supporting code for apps that can be shared.
        shared/ ;; Each items in here can be selectively shared in named 'user-groups' in item properties. ;; Maybe call common/ ?
            .config
            app/
                com.android.studio/
                com.google.chrome/
                    .cache
                io.atom/
            doc/
            game/
                .leaderboard
                net.minecraft/
            media/
            theme/ ;; Default GUI options here? Kinda like 'launchers' in Android?
        user/
            .config.anon
            my-username/
                .config
                .cache
                app/
                    com.android.studio/.config ;; Each user may have their own specific configurations.
                    com.google.chrome/.config ;; Apps check user's app/ first, then shared/ for .config or .config.anon.
                    io.atom/.config
                doc/
                game/
                    net.minecraft/
                mail/ ;; Maybe?
                media/

Notes on above example:

- The main focus should be making things easier for the end user. 
- No default sub-groups for media/ because OS should be able to display it all organized by itself. A lot of times, I want images and videos in the same group for easier organization for myself. Also, different GIFs could be mapped either to image or video.
- There is no specific /root/ group because the /os/ group would cover that.
- This particular file system doesn't contain a place for multiple OS because ideally, there would only be one (though, likely won't happen)
- Downloads could go to proper location or temp/
- Hmm, 'game' really is an 'app'... Maybe make it 'app-game/' or 'app/game/'? User config file could point to it's actual location.
- Instead of a specific desktop/ group, the username root can have the files shown in main page GUI (other groups optionally also).
