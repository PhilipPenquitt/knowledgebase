# Fehler

## Tastatur wird falsch erkannt

Ich kenne die Ursache nicht, aber es kann nach Updates vorkommen, dass Mac die ^
und < vertauscht. Ich vermute, dass Mac Intern die Tastatur als ANSI Model
erkennt, obwohl es als ISO angegeben wird.

### Lösung

[Lösung](https://apple.stackexchange.com/questions/239395/and-keys-are-swapped)

```text
To change it to the right type:

Open System Preferences > Keyboard, select the Keyboard tab and then 'Change Keyboard Type...'

This opens the Keyboard Setup Assistant.

Follow the prompt 'Press the key to the right of the shift key on the left side.'

It should now have detected and selected 'ISO' as your standard. If not select it and click 'OK'. 'ISO' is generally used for European keyboards.

If this doesn't solve it, you can try manually resetting the settings file:

Delete /Library/Preferences/com.apple.keyboardtype.plist
Restart
When the keyboard assistent pops up. press the key like it says and select ISO (European)
```
