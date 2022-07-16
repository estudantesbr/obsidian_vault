+ SaveToYourLibraryIcon.png tem que ser o "coraçãozinho" de adicionar do Spotify
+ https://superuser.com/questions/1263856/is-there-a-keyboard-shortcut-for-save-to-your-library-in-spotifys-desktop-app/1280856?noredirect=1#comment1892017_1280856

```javascript
; Control+Shift+Win+F1
^+#F1:: SendInput {Media_Play_Pause}

; Control+Shift+Win+F2
^+#F2:: SendInput {Media_Prev}

; Control+Shift+Win+F3
^+#F3:: SendInput {Media_Next}
    
; Control+Shift+Win+F4
^+#F4:: SaveSongToSpotifyLibrary()


SaveSongToSpotifyLibrary() {
    spotify := "ahk_exe spotify.exe"
    if WinExist(spotify) {
        ; Store starting window ID and mouse position.
        MouseGetPos x, y, startingWinId

        ; Activate Spotify.
        WinActivate %spotify%
        WinWaitActive %spotify%

        saveToYourLibraryIcon = %A_WorkingDir%\apps\SpotifyController\SaveToYourLibraryIcon.png
        ImageSearch FoundX, FoundY, 0, 0, A_ScreenWidth, A_ScreenHeight, %saveToYourLibraryIcon%
        if (ErrorLevel = 0) {
            Click %FoundX%, %FoundY%

        } else if (ErrorLevel = 2) {
            MsgBox % "Problem conducting image search. Is the saveToYourLibraryIcon in the correct location?"

        } else if (ErrorLevel = 1) {
            MsgBox % "Unable to save song. Can't find the Add button."
        }

        ; Restore original window and mouse position.
        WinActivate ahk_id %startingWinId%
        MouseMove %x%, %y%
    }
}
```

