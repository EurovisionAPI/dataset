# ❤️ Unofficial Eurovision Song Contest Dataset ❤️

This site is a freely accessible dataset that contains information about the participants and votes of all editions of the Eurovision Song Contest and Junior Eurovision Song Contest.

The dataset will be updated annually with the results of the contest, covering all editions from its inception in 1956 to the present. For Junior Eurovision, the dataset includes results starting from its first edition in 2003.

The data are obtained from the [ESC Home](https://eschome.net/), [Eurovision World](https://eurovisionworld.com), [Eurovision LOD](https://so-we-must-think.space/greenstone3/eurovision-library/collection/eurovision/page/about), [Logopedia](https://logos.fandom.com/), [Ogaespain](https://www.ogaespain.com/), [Tunebat](https://tunebat.com/), [Musictax](https://musicstax.com/) and [Chordify](https://chordify.net/) websites.

You can find more information on the [website](https://eurovisionapi.runasp.net/).

## 🗂️ Dataset structure

### Data folder (`data/`)

It is organized into three main sections:

| Path             | Contents                                               |
| ---------------- | ------------------------------------------------------ |
| `countries.json` | Country code → country name dictionary                 |
| `senior/`        | Eurovision Song Contest editions (1956–present)        |
| `junior/`        | Junior Eurovision Song Contest editions (2003–present) |

#### Countries (countries.json)

A simple `string → string` dictionary mapping **country code** to **country name**, for every country that has ever participated.

```json
{
  "ES": "Spain",
  "IT": "Italy",
  ...
}
```

This code (`ES`, `IT`, etc.) is the reference used throughout the rest of the files (`country` field, `votes` keys, etc.).

### Contest editions

Both `senior/` and `junior/` follow exactly the same directory layout. Each directory contains the editions of the corresponding contest.

```
2024/
├── contest.json
├── rounds/
│   ├── final.json
│   ├── semifinal1.json
│   └── semifinal2.json
└── contestants/
    ├── 00_cy/
    ├── 01_rs/
    └── ...
```

#### Contest (`contest.json`)

Contains the data describing the contest.

| Attribute         | Type     | Description                                                                                     |
| ----------------- | -------- | ----------------------------------------------------------------------------------------------- |
| `arena`           | string   | Venue where it was held                                                                         |
| `city`            | string   | Host city                                                                                       |
| `country`         | string   | Host country code                                                                               |
| `intendedCountry` | string   | If not `null`, the code of the country that should have hosted but couldn't (e.g. Ukraine 2023) |
| `slogan`          | string   | Edition's slogan                                                                                |
| `presenters`      | string[] | Presenters                                                                                      |
| `broadcasters`    | string[] | Host broadcasters                                                                               |

### Contestants folder (`contestants/`)

Each contestant has its own directory.

```
contestants/
├── 00_it/
│   ├── contestant.json
│   └── lyrics/
├── 01_es/
└── ...
```

The name of each contestant's directory follows the following format:

```
<placement-order>_<country-code>
```

#### Contestant (`contestant.json`)

Contains all metadata related to a contestant.

| Attribute       | Type     | Description                                                                                        |
| --------------- | -------- | -------------------------------------------------------------------------------------------------- |
| `id`            | integer  | Contestant ID (used later in `Performance`)                                                        |
| `country`       | string   | Code of the country represented                                                                    |
| `artist`        | string   | Singer/group name                                                                                  |
| `song`          | string   | Song title                                                                                         |
| `videoUrls`     | string[] | YouTube video links                                                                                |
| `bpm`           | integer  | Beats per minute                                                                                   |
| `tone`          | string   | Song's key and scale                                                                               |
| `artistPeople`  | string[] | The real name of the artist; if the artist is a group, the names correspond to the group’s members |
| `backings`      | string[] | Backing vocalists                                                                                  |
| `dancers`       | string[] | Dancers                                                                                            |
| `composers`     | string[] | Composers                                                                                          |
| `lyricists`     | string[] | Lyricists                                                                                          |
| `writers`       | string[] | Writers                                                                                            |
| `conductor`     | string   | Orchestra conductor                                                                                |
| `stageDirector` | string   | Stage director                                                                                     |
| `broadcaster`   | string   | Country's broadcaster                                                                              |
| `commentators`  | string[] | Country's commentators                                                                             |
| `spokesperson`  | string   | Country's spokesperson                                                                             |
| `jury`          | string[] | National selection jury                                                                            |

---

#### Lyrics folder (`lyrics/`)

Lyrics are stored as plain text files. The paragrpahs are separated by double line break (`\n\n`).

The filename consists of a prefix followed by the language or languages involved. When lyrics contain two or more languages, they are separated by a comma (,).

```
lyrics/
├── o_english.txt
├── t_spanish.txt
├── t_bulgarian_1.txt
├── t_bulgarian_2.txt
├── t_serbian_srpski.txt
├── v_italian.txt
├── v_italian,spanish.txt
└── ...
```

| Prefix | Meaning                                                                                      |
| ------ | -------------------------------------------------------------------------------------------- |
| `o_`   | Original lyrics                                                                              |
| `t_`   | Translation                                                                                  |
| `v_`   | Version adapted to the language (the translation of the lyrics can differ from the original) |

The language portion of the filename follows these rules:

- Multiple languages are separated by a comma (,).
  - v_italian,spanish.txt — a version containing both Italian and Spanish.
- Multiple distinct lyrics in the same language are numbered using an underscore (\_) followed by the number.
  - t_bulgarian_1.txt — first Bulgarian translation.
  - t_bulgarian_2.txt — second Bulgarian translation.
- Language specializations or variants are also separated using an underscore (\_).
  - t_serbian_srpski — Serbian translation, specifically the Srpski variant.

The general filename format is:

```
<prefix><language>[,<language>...][_number].txt
```

---

### Rounds folder (`rounds/`)

Contains one file for each show. Each file stores the performances, results and voting information for that round.

```
rounds/
├── final.json
├── semifinal1.json
└── semifinal2.json
```

The name of each round is determined by the filename. The filename (without the .json extension) is used as the round name.
The following naming conventions are used:

- final.json → final
- semifinal.json → semifinal (2004–2007)
- semifinal1.json → semifinal1 (from 2008 onward)
- semifinal2.json → semifinal2 (from 2008 onward)

#### `Round`

| Attribute       | Type            | Description                     |
| --------------- | --------------- | ------------------------------- |
| `date`          | string          | Date in UTC                     |
| `time`          | string          | Time in UTC                     |
| `performances`  | `Performance[]` | Results for the contestants     |
| `disqualifieds` | int[]           | IDs of contestants disqualified |

#### `Performance`

| Attribute      | Type      | Description                                |
| -------------- | --------- | ------------------------------------------ |
| `contestantId` | integer   | Contestant ID (references `Contestant.id`) |
| `running`      | integer   | Running order position                     |
| `place`        | integer   | Ranking position                           |
| `scores`       | `Score[]` | Points and votes received                  |

#### `Score`

| Attribute | Type                          | Description                                                                           |
| --------- | ----------------------------- | ------------------------------------------------------------------------------------- |
| `name`    | string                        | Source of the points: `total`, and from 2016 onward also `tele` and `jury` separately |
| `points`  | integer                       | Total points earned                                                                   |
| `votes`   | `Dictionary<string, integer>` | Votes received from each country (key = country code)                                 |

_(In junior, `Score` doesn't have the per-country `votes` breakdown like in senior.)_

---

### Images folder (`images/`)

It contains the logo image files for each contest edition (senior and junior). Files are organized by edition and then by year.

```
logos/
├── junior/
│   ├── 1956.png
│   ├── 1957.png
│   └── ...
└── senior/
    ├── 2003.png
    ├── 2004.png
    └── ...
```

## 📚 Dataset Applications

If you use this dataset in your own research, project, publication or any other work, please let me know so it can be added to this list. This helps keep track of how the dataset is being used and gives visibility to projects that build upon it.

- **[Graph Drawing 2026](https://graphdrawing.github.io/gd2026/)** — _34th International Symposium on Graph Drawing and Network Visualization_, 2026.

## 🙌 Contributions welcome!

We truly appreciate any contribution to improve the dataset!
If you notice any missing data, feel free to help by adding it.

To contribute:

1. Fork the repository.
2. Make the changes in your fork. Do not edit the README, it will be automatically updated.
3. Open a Pull Request (PR). In the PR description, please include the source of the data so we can verify its accuracy.

### ❓ Known missing data

#### 👦👧 Junior

<!-- JUNIOR-START -->

No missing data

<!-- JUNIOR-END -->

#### 🧔👩 Senior

<!-- SENIOR-START -->

No missing data

<!-- SENIOR-END -->
