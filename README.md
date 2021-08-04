# Dactyl manuform build log

## BOM
| Item                                         | Price | Location                                                                                                                       |
| --                                           | --    | --                                                                                                                             |
| 3d printed case printed with local provider. | 30 €  | https://www.treatstock.com                                                                                                     |
| 2x Pro Micro                                 | 12 €  | https://www.aliexpress.com/snapshot/0.html?spm=a2g0s.9042647.0.0.4de34c4dnSNGrW&orderId=8135886042885033&productId=32768308647 |
| 60 x 1N4148 Diode                            | 2 €   | https://www.ic-elect.si/dioda-1n4148-do-35-4ns-100v-0-5a-34483.html                                                            |
| Outemu brown + keycasp                       | 27 €  | Canibalized Redragon K630                                                                                                      |


## Case

I've used carbonfet's project (https://github.com/carbonfet/dactyl-manuform). It uses clojure to generate .scad files that can be opened with OpenSCAD and exported to .stl. While Pyhton project (https://github.com/joshreve/dactyl-keyboard) looks promising, at the time it looked a bit inmature. 

I went with 4x6 design - diff can be found [here](https://github.com/TomazKarer/dactyl_buildlog/blob/main/case.diff).

.stl files where uploadet to Treatstock. I've also uploaded pregenerated pro micro holder.stl, but forgot to have it printed twice. I choose the cheapest provider that offered the print for 12 € but the offer was rejected :). So after waiting for a few days for a refund went with the next in line, which also provided delivery free of charge. After two days the print was done and I was able to inspect it over the photos provided. It arrived two days after in a mail. It looked fine but the Pro micro holder didn't seem to fit.

## Switches and keycaps

I've bought a Red Dragon K630 keyboard for measly 27 € a few months ago. It looked like a decent 60 % keyboard and it uses Fn key for accesing keys like Insert, End etc. The problem that this keyboard have, is it doesn't register additional combinations for example Shift + Insert (Fn + [{). Since this makes is unusable for my use cases, I've tried to returned it but got fed up with Slovenian store making me jump through hoops and ignoring emails.

It uses Outemu Brown hot swap switches and they seemed perfect for Dactyl I intended to build. I've decided to go against hot swap option for now.

Since there were 61 keys this also determined layout for Dactyl.

# Electronics


