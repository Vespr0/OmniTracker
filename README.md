# OmniTracker

Multi-purpose time trackers for your notes!

![A screenshot of the plugin in action, where you can see an active time tracker for a project](https://raw.githubusercontent.com/Ellpeck/ObsidianOmniTracker/master/screenshot.jpg)

# 🤔 Usage

To get started tracking your time with OmniTracker, open up the note that you want to track your time in. Move the cursor to the area you want the tracker to reside in, and then open your command palette and execute the `OmniTracker: Insert Time Tracker` command.

When switching to live preview or reading mode, you will now see the time tracker you just inserted! Now, simply name the first segment (or leave the box empty if you don't want to name it) and press the **Start** button. Once you're done with the thing you were doing, simply press the **End** button and the time you spent will be saved and displayed to you in the table.

Need help using the plugin? Feel free to join the Discord server!

[![Join the Discord server](https://ellpeck.de/res/discord-wide.png)](https://link.ellpeck.de/discordweb)

## 🔍 Tracker Data in Dataview

OmniTracker has a public API that can be used with [Dataview](https://blacksmithgu.github.io/obsidian-dataview/), specifically [DataviewJS](https://blacksmithgu.github.io/obsidian-dataview/api/intro/), which can be accessed using the following code:

```js
dv.app.plugins.plugins["omnitracker"].api;
```

The following is a short example that uses DataviewJS to load all trackers in the vault and print the total duration of each tracker. You can also find this example in action [in the test vault](https://github.com/Ellpeck/ObsidianOmniTracker/blob/master/test-vault/dataview-test.md?plain=1).

```js
// get the time tracker plugin api instance
let api = dv.app.plugins.plugins["omnitracker"].api;

for (let page of dv.pages()) {
    // load trackers in the file with the given path
    let trackers = await api.loadAllTrackers(page.file.path);

    if (trackers.length) dv.el("strong", "Trackers in " + page.file.name);

    for (let { section, tracker } of trackers) {
        // print the total duration of the tracker
        let duration = api.getTotalDuration(tracker.entries);
        dv.el("p", api.formatDuration(duration));
    }
}
```

A full list of the functions exposed through the API can be found [in the code](https://github.com/Ellpeck/ObsidianOmniTracker/blob/master/src/main.ts#L8-L16). Proper documentation for the API will be added in the future.

# 👀 What it does

A time tracker is really just a special code block that stores information about the times you pressed the Start and End buttons on. Since time is tracked solely through timestamps, you can switch notes, close Obsidian or even shut down your device completely while the tracker is running! Once you come back, your time tracker will still be running.

The tracker's information is stored in the code block as JSON data. The names, start times and end times of each segment are stored. They're displayed neatly in the code block in preview or reading mode.

# ✨ What's New

- **Improved Controls**: Consolidated redundant table row controls into sleek global buttons.
- **Selective Tracking**: You can now select specific rows to target with global play/pause/stop controls.
- **Artificial Time**: Introduced the ability to manually log time in minutes for segments that weren't tracked in real-time.
- **Enhanced UI**: Visual highlights for selected rows and improved layout alignment.
- **Pause & Resume**: You can now pause your tracking sessions and resume them later within the same segment.
- **Persistent Pause State**: The paused state is saved directly into your notes.

# 🙏 Acknowledgements

This plugin was originally created by [Ellpeck](https://ellpeck.de) as "Super Simple Time Tracker". I (Vespr0) have refactored and renamed it to OmniTracker to add new features like the pause button.

If you want to support the original creator, you can do so through their website:
[![Support Ellpeck](https://ellpeck.de/res/generalsupport-wide.png)](https://ellpeck.de/support)
