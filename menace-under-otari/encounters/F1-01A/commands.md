# Floor 1, Encounter 01A

Fight in the entrance to the underground.

## Setup

### Configuration

**Battle Plan**

```
!bplan new "Menace F1-01A"
!bplan add "Menace F1-01A" !i add 0 Lair -p 99
!bplan add "Menace F1-01A" !i madd "Plague-ridden Rat" -name "R#" -n 4 -group Rats rollhp
```

**Map**

```
!map -attach Lair -mapsize 8x9 -loadjson https://raw.githubusercontent.com/bwreid/pbp/main/menace-under-otari/encounters/F1-01A/map.json
!map -t R1|G5 -t R2|G6 -t R3|H5 -t R4|H6
!map -savebattle "Menace: F1-01A"
```

### Loading

## Monsters

### Plague-ridden Rat

#### Configuration

These will need to be added in the [Avrae Dashboard](https://avrae.io/dashboard/characters) to an Unnamed character.

**Festering Bite**

```yaml
- _v: 2
  name: Festering Bite
  automation:
    - type: target
      target: each
      effects:
        - type: attack
          hit:
            - type: damage
              damage: 1d4 + 2 [piercing]
            - type: save
              stat: con
              fail:
                - type: ieffect2
                  name: Poisoned
                  duration: "10"
                  effects:
                    attack_advantage: "-1"
                    check_dis:
                      - all
                  buttons:
                    - automation:
                        - type: target
                          target: self
                          effects:
                            - type: save
                              stat: dex
                              fail: []
                              success:
                                - type: remove_ieffect
                                  removeParent: if_no_children
                      label: Resist Condition
                      verb: attempts to resist Condition
                      defaultDC: lastSaveDC
                  end: true
              success: []
              dc: "10"
            - type: damage
              damage: 1 [poison]
              fixedValue: true
          miss: []
          attackBonus: "4"
    - type: text
      text: "*Melee Weapon Attack:* +4 to hit, reach 5 ft., one target. *Hit:* 4 (1d4
        + 2) piercing damage."
  thumb: https://i.imgur.com/nioApmH.png

```

#### Actions

**Move**

```
!map -t TARGET -move LOCATION
```

**Festering Bite**

```
!a festering -t TARGET -title "RAT attacks with a festering bite!"
```