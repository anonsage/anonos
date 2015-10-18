The most-recently-used windows are remembered. There is a separate list for each desktop.

The following window attributes will be remembered for each app:

    window-attribute [
      width
      height
      x
      y
      snap
      screen
      app-name
    ]

When a app/window is open, there will be additional attributes available to query:

    window-attribute [
      is-allow-sound
      is-playing-sound
      is-active
    ]

Each window is resizeable and moveable by user, but not smaller than the title bar size and never off the screen. This is to ensure the app can still be found and resized again.
