# Dactyl manuform build log

## BOM
| Item                                         | Price | Location                                                                                                                       |
| --                                           | --    | --                                                                                                                             |
| 3d printed case printed with local provider. | 30 €  | https://www.treatstock.com                                                                                                     |
| 2x Pro Micro                                 | 12 €  | https://www.aliexpress.com/snapshot/0.html?spm=a2g0s.9042647.0.0.4de34c4dnSNGrW&orderId=8135886042885033&productId=32768308647 |
| 60 x 1N4148 Diode                            | 2 €   | https://www.ic-elect.si/dioda-1n4148-do-35-4ns-100v-0-5a-34483.html                                                            |
| Conductive Copper Tape                       | 8 €   | local electronics store                                                                                                        |
| TRS connector                                | 3 €   | local electronics store                                                                                                        |
| Color cables (40 pieces)                     | 3 €   | local electronics store                                                                                                        |
| Outemu brown + keycasp                       | 27 €  | canibalized Redragon K630                                                                                                      |

| Tool              | Price | Location |
| --                | --    | --      |
| Soldering station | ?     |          |
| Multimeter        | ?     |          |


### Case

I've used carbonfet's project (https://github.com/carbonfet/dactyl-manuform). It uses clojure to generate .scad files that can be opened with OpenSCAD and exported to .stl. While Pyhton project (https://github.com/joshreve/dactyl-keyboard) looks promising, at the time it looked a bit inmature. 

I went with 4x6 design - diff can be found [here](https://github.com/TomazKarer/dactyl_buildlog/blob/main/case.diff).

.stl files where uploadet to Treatstock. I've also uploaded pregenerated pro micro holder.stl, but forgot to have it printed twice. I choose the cheapest provider that offered the print for 12 € but the offer was rejected :). So after waiting for a few days for a refund went with the next in line, which also provided delivery free of charge. After two days the print was done and I was able to inspect it over the photos provided. It arrived two days after in a mail. It looked fine but the Pro micro holder didn't seem to fit.

### Switches and keycaps

I've bought a Red Dragon K630 keyboard for measly 27 € a few months ago. It looked like a decent 60 % keyboard and it uses Fn key for accesing keys like Insert, End etc. The problem that this keyboard have, is it doesn't register additional combinations for example Shift + Insert (Fn + [{). Since this makes is unusable for my use cases, I've tried to returned it but got fed up with Slovenian store making me jump through hoops and ignoring emails.

It uses Outemu Brown hot swap switches and they seemed perfect for Dactyl I intended to build. I've decided to go against hot swap option for now.

Since there were 61 keys this also determined layout for Dactyl. While I didn't have much (any!) experience with 40 % or similar design I still ventured this way. 4x6 Manuform layout has a large thumb cluster and the rest of the keys all are near home row position. The lack of keys should be compensated with 6 key thumb cluster. Surely not optimal since all cluster keys are not easy to access but it was worth a try.

### Electronics

As far as electronic components go, I've bought a few from local electronic stores and other from Aliexpress. I could've saved a few Euros if would have ordered everything from Alis, but the difference is little. 


I already owned a soldering station - I've got no idea how much did it cost.

## Build process

After recieving the case a month passed until I've found time and energy (and willpower - raising twins is an adventure :)) to continue. 

After reading a couple tutorials/build logs I've decided to start with conductive copper tape for connecting vertical lines. This wasn't very hard - the important thing is to use copper tape first and only after applying copper tape continue with soldering the diodes. During aplying the tape be careful not to tear it in half - only tear a small hole for the pin. Soldering pins was not a big deal. Afterwards I've checked connections with multimeter (the impendance of pressed switch should be near zero) and it seemed fine.

The diodes connect the switches horizontaly. You need to be careful regarding orientation of the diodes, it does matters. The soldering took some time but it was not hard and it took maybe an hour.

Color cables had female connectors on both side. I cut off connector from one side of the cable and soldered it to horizontal line connecting diodes and vertical line of copper tape. The other part of the cable was easy to connect to Pro Micro on which I've soldered the pin heads.

The hardware part was done. It was not in any way professional job. I've found out that there was no space in keyboard case for all the cables, diodes with their connections and Pro Micros. The solders were brittle and prone to disconnections because the cables I've used were not sutable for the project. Also the Pro Micros with theirs pin heads were too large for their enclosures. 

But everyting seemed to work and I've decided to continue. Even though at that point I knew that the end project will not be pretty I wanted to find out if it will work at all - I continued with software.

## Software 

QMK is open source project for developing firmware for keyboards (mostly). It's written in C with an extensive documentation on every subject. Since I am a developer by profession I thought that costumising software would be a easy task. Especially since the hardware part went mostly smoothly, even though I've done it for the first time without any previous experience. Well, it wasn't as easy as I've thought. 

First part is obtaining the software - forking the QMK is easy and compiling and flashing the handwired/dactyl_manuform/4x6 example was not be complicated. Here I've stumbled over first roadblock - the keyboards left and right part did not seem to communicate with each other. I've double checked all conections and blamed the software. After a few hours trying to solve the issue I've found out the cause - spotty connection between the keyboard parts.

When I resolved this problem flashing went smoothly. Customising layout was easy as well.

My native language is Slovenian. Slovenians use basic Latin alphabet plus three letters - č, š and ž called šumniki. But the Keyboard layout is very different from standard US layout especialy when it comes to characters used developing computer software. Because of this fact and the fact, that there is very limited selection of mechanical keyboards with Slovene layout I've been using an [EurKEY](https://eurkey.steffen.bruentjen.eu/) layout. It advertises itself as layout for European, Coders and Translators. It mostly deliverers this promise - it is based on US layout with its convenience for typing the special charactersand availability of most of the characters of vast European languages. While the Slovenes 'šumniki' still feel as second class citizens they do work with an combo of AltGr+Shift+6 and c/s/z.

Without thinking much about different layouts and theirs implementation I've naively thought that setting a custom keycode for šumniks on one of the layers would suffice. But different layouts of course don't work this way. The keycodes sent are the same for all the layouts, and the OS decides, based on the chosen layout, which character to display. Learning this I've thought about using Slovene layout and remmaping all the different keys. But differences between the US and Slovene layout are not subtle. For example ; and : - US layout uses one key, Slovene two. 

I've solved my issue with this QMK macro:

`
enum custom_keycodes {
	SUMNIK_S = SAFE_RANGE,
	SUMNIK_C,
	SUMNIK_Z
	};


bool process_record_user(uint16_t keycode, keyrecord_t *record) {
    switch (keycode) {
    case SUMNIK_S:
        if (record->event.pressed) {
	    SEND_STRING(SS_DOWN(X_RALT) SS_DOWN(X_LSHIFT) "6" SS_UP(X_RALT) SS_UP(X_LSHIFT) "s");
        } else {
        }
        break;
    case SUMNIK_C:
        if (record->event.pressed) {
	    SEND_STRING(SS_DOWN(X_RALT) SS_DOWN(X_LSHIFT) "6" SS_UP(X_RALT) SS_UP(X_LSHIFT) "c");
        } else {
        }
        break;
    case SUMNIK_Z:
        if (record->event.pressed) {
	    SEND_STRING(SS_DOWN(X_RALT) SS_DOWN(X_LSHIFT) "6" SS_UP(X_RALT) SS_UP(X_LSHIFT) "z");
        } else {
        }
        break;
    }
    return true;
};
`

It sends a macro of key codes that mimic the combination used for šumnik in EurKEY layout. 

This solved the last issue preventing me from using the keyboard for work. The layout is still a work in progres - it can be accessed here (branch tk).
