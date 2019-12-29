年底把系统升级到了Catalina, 在整理密码的时候发现网上的脚本都不能拿过来直接用，只能自己修改了一版出来。

以下脚本直接放入Apple script，能从icloud钥匙串导出存储的账户密码到文本文件。

导出前可以看看需要导出多少密码，设置以下repeat次数就行。
说不定这个脚本明年年底又能用得到。





    repeat 2 times
	delay 0.2
	tell application "Keychain Access"
		activate
		tell application "System Events"
			tell application process "Keychain Access"
				set _selection to value of attribute "AXFocusedUIElement"
				tell _selection to perform action "AXShowMenu"
				delay 0.2
				key code 125
				delay 0.2
				keystroke return
				delay 0.2
				entire contents
				keystroke "PASSWORD"
				delay 0.2
				keystroke return
				delay 0.2
			end tell
		end tell
	end tell

	--Switch to TextEdit
	delay 0.2
	tell application "TextEdit"
		activate
		tell application "System Events"
			-- Press ⌘V
			delay 0.2
			keystroke "v" using command down

			-- Type a comma
			delay 0.2
			keystroke ","

		end tell
	end tell

	--Switch to Keychain
	tell application "Keychain Access"
		activate
		tell application "System Events"
			delay 0.2
			keystroke return
			-- Press Tab x1
			delay 0.1
			keystroke tab
			-- Press ⌘C
			delay 0.2
			keystroke "c" using command down
		end tell
	end tell

	--Switch to TextEdit
	delay 0.2
	tell application "TextEdit"
		activate
		tell application "System Events"
			-- Press ⌘V
			delay 0.2
			keystroke "v" using command down
			-- Type ','
			delay 0.2
			keystroke ","
		end tell
	end tell


	--Switch to Keychain
	tell application "Keychain Access"
		activate
		tell application "System Events"
			delay 0.1
			keystroke tab
			delay 0.1
			keystroke tab
			-- Press ⌘C to copy item title
			delay 0.2
			keystroke "c" using command down
			delay 0.3
			keystroke "w" using command down
			-- Go to next keychain item
			delay 0.2
			key code 125
		end tell
	end tell


	--Switch to TextEdit
	delay 0.2
	tell application "TextEdit"
		activate
		tell application "System Events"
			-- Press ⌘V
			delay 0.2
			keystroke "v" using command down

			-- Press Return
			delay 0.2
			keystroke return
		end tell
	end tell
    end repeat
    end run
