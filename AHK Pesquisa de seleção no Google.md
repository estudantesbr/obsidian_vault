Here is a Google Search of Highlighted text that allows you to do a web search (Win+G), an image search (Ctrl+Win+G) or a map search (Alt+Win+G).

```py
; Google Search
; Fanatic Guru
; 2014 10 14
; Version: 1.2
;
; Google Search of Highlighted Text
;
;{-----------------------------------------------
; If Internet Explorer is already running it will add search as new tab
;}

; INITIALIZATION - ENVIROMENT
;{-----------------------------------------------
;
#NoEnv  ; Recommended for performance and compatibility with future AutoHotkey releases.
SendMode Input  ; Recommended for new scripts due to its superior speed and reliability.
SetWorkingDir %A_ScriptDir%  ; Ensures a consistent starting directory.
#SingleInstance force  ; Ensures that only the last executed instance of script is running
;}

; AUTO-EXECUTE
;{-----------------------------------------------
;
RegRead, ProgID, HKEY_CURRENT_USER, Software\Microsoft\Windows\Shell\Associations\UrlAssociations\http\UserChoice, Progid
Browser := "YOURBROWSER.exe"
if (ProgID = "ChromeHTML")
   Browser := "chrome.exe"
if (ProgID = "FirefoxURL")
   Browser := "firefox.exe"
;
;}-----------------------------------------------
; END OF AUTO-EXECUTE

; HOTKEYS
;{-----------------------------------------------
;
#g::    ; <-- Google Web Search Using Highlighted Text
   Search := 1
   Gosub Google
return

^#g::    ; <-- Google Image Search Using Highlighted Text
   Search := 2
   Gosub Google
return

!#g::    ; <-- Google Map Search Using Highlighted Text
   Search := 3
   Gosub Google
return
;}

; SUBROUTINES
;{-----------------------------------------------
;
Google:
   Save_Clipboard := ClipboardAll
   Clipboard := ""
   Send ^c
   ClipWait, .5
   if !ErrorLevel
      Query := Clipboard
   else
      InputBox, Query, Google Search, , , 200, 100, , , , , %Query%
   StringReplace, Query, Query, `r`n, %A_Space%, All
   StringReplace, Query, Query, %A_Space%, `%20, All
   StringReplace, Query, Query, #, `%23, All
   Query := Trim(Query)
   if (Search = 1)
      Address := "http://www.google.com/search?hl=en&q=" Query ; Web Search
   else if (Search = 2)
      Address := "http://www.google.com/search?site=imghp&tbm=isch&q=" Query ; Image Search
   else
      Address := "http://www.google.com/maps/search/" Query ; Map Search
   if (Browser = "iexplore.exe")
   {
      Found_IE := false
      For wb in ComObjCreate("Shell.Application").Windows
         If InStr(wb.FullName, "iexplore.exe")
         {
            Found_IE := true
         	break
         }
      if Found_IE
         wb.Navigate(Address, 2048)
      else
      {
         wb := ComObjCreate("InternetExplorer.Application")
         wb.Visible := true
         wb.Navigate(Address)
      }
   }
   else
      Run, %browser% %Address%
   Clipboard := Save_Clipboard
   Save_Clipboard := ""
return
;}
```