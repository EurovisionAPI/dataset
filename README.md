# ❤️ Unofficial Eurovision Song Contest Dataset ❤️

This site is a freely accessible dataset that contains information about the participants and votes of all editions of the Eurovision Song Contest and Junior Eurovision Song Contest.

The dataset will be updated annually with the results of the contest, covering all editions from its inception in 1956 to the present. For Junior Eurovision, the dataset includes results starting from its first edition in 2003.

The data is obtained from the [ESC Home](https://eschome.net/), [Eurovision World](https://eurovisionworld.com), [Eurovision LOD](https://so-we-must-think.space/greenstone3/eurovision-library/collection/eurovision/page/about), [Logopedia](https://logos.fandom.com/), [Ogaespain](https://www.ogaespain.com/), [Tunebat](https://tunebat.com/), [Musictax](https://musicstax.com/) and [Chordify](https://chordify.net/) websites.

You can find more information on the [website](https://eurovisionapi.runasp.net/).

## Dataset structure 🚧 Work in progress 🚧
### Data folder (`data/`)
It is organized into three main sections:

| Path | Contents |
|---|---|
| `countries.json` | Country code → country name dictionary |
| `senior/` | Eurovision Song Contest editions (1956–present) |
| `junior/` | Junior Eurovision Song Contest editions (2003–present) |

#### Countries (countries.json)
A simple `string → string` dictionary mapping **country code** to **country name**, for every country that has ever participated.

```json
{
  "es": "Spain",
  "it": "Italy",
  ...
}
```

This code (`es`, `it`, etc.) is the reference used throughout the rest of the files (`country` field, `votes` keys, etc.).

### Contest editions

Both `senior/` and `junior/` follow exactly the same directory layout. Each directory represents one contest edition.

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

| Attribute | Type | Description |
|---|---|---|
| `arena` | string | Venue where it was held |
| `city` | string | Host city |
| `country` | string | Host country code |
| `intendedCountry` | string | If not `null`, the code of the country that should have hosted but couldn't (e.g. Ukraine 2023) |
| `slogan` | string | Edition's slogan |
| `presenters` | string[] | Presenters |
| `broadcasters` | string[] | Host broadcasters |

### Contestants (`contestants/`)
Each contestant has its own directory.

```
contestants/
├── 00_ch/
│   ├── contestant.json
│   └── lyrics/
├── 01_hr/
└── ...
```

The directory name follows the format:

```
<running-order>_<country-code>
```

Example:

```
21_es
```


### contestant.json
Contains all metadata related to a contestant.

| Attribute | Type | Description |
|---|---|---|
| `id` | integer | Contestant ID (used later in `Performance`) |
| `country` | string | Code of the country represented |
| `artist` | string | Singer/group name |
| `song` | string | Song title |
| `videoUrls` | string[] | YouTube video links |
| `bpm` | integer | Beats per minute |
| `tone` | string | Song's key and scale |
| `artistPeople`  | string[] | The real name of the artist; if the artist is a group, the names correspond to the group’s members |
| `backings` | string[] | Backing vocalists |
| `dancers` | string[] | Dancers |
| `jury` | string[] | National selection jury |
| `composers` | string[] | Composers |
| `lyricists` | string[] | Lyricists |
| `writers` | string[] | Writers |
| `conductor` | string | Orchestra conductor |
| `stageDirector` | string | Stage director |
| `broadcaster` | string | Country's broadcaster |
| `spokesperson` | string | Country's spokesperson |
| `commentators` | string[] | Country's commentators |

---

### lyrics/
Lyrics are stored as plain text files.

```
lyrics/
├── o_english.txt
├── t_spanish.txt
├── t_french.txt
├── v_italian.txt
└── ...
```

| Prefix | Meaning |
|---|---|
| `o_` | Original lyrics |
| `t_` | Translation |
| `v_` | Translation |

---

### rounds/
Contains one JSON file for each show.

```
rounds/
├── final.json
├── semifinal1.json
└── semifinal2.json
```

Each file stores the performances, results and voting information for that round.

---



### `Contestant`

Each song/entry of the edition.

| Attribute | Type | Description |
|---|---|---|
| `id` | integer | Contestant ID (used later in `Performance`) |
| `country` | string | Code of the country represented |
| `artist` | string | Singer/group name |
| `song` | string | Song title |
| `lyrics` | `Lyrics[]` | Original lyrics + translations |
| `videoUrl` | string[] | YouTube video links |
| `tone` | string | Song's key and scale |
| `bpm` | integer | Beats per minute |
| `dancers` | string[] | Dancers |
| `backings` | string[] | Backing vocalists |
| `jury` | string[] | National selection jury |
| `composers` | string[] | Composers |
| `lyricists` | string[] | Lyricists |
| `writers` | string[] | Writers |
| `conductor` | string | Orchestra conductor |
| `stageDirector` | string | Stage director |
| `broadcaster` | string | Country's broadcaster |
| `spokesperson` | string | Country's spokesperson |
| `commentators` | string[] | Country's commentators |

### `Lyrics`

| Attribute | Type | Description |
|---|---|---|
| `languages` | string[] | Languages present in the lyrics |
| `title` | string | Song title |
| `content` | string | Full lyrics (paragraphs separated by `\n\n`) |

### `Round`

| Attribute | Type | Description |
|---|---|---|
| `name` | string | `final`, `semifinal` (2004–2007), or `semifinal1`/`semifinal2` (from 2008 onward) |
| `date` | string | Date in UTC |
| `time` | string | Time in UTC |
| `performances` | `Performance[]` | Results for the contestants in that round |
| `disqualifieds` | int[] | IDs of contestants disqualified in that round |

### `Performance`

| Attribute | Type | Description |
|---|---|---|
| `contestantId` | integer | Contestant ID (references `Contestant.id`) |
| `running` | integer | Running order position |
| `place` | integer | Final ranking position |
| `scores` | `Score[]` | Points and votes received |

### `Score`

| Attribute | Type | Description |
|---|---|---|
| `name` | string | Source of the points: `total`, and from 2016 onward also `tele` and `jury` separately |
| `points` | integer | Total points earned |
| `votes` | `Dictionary<string, integer>` | Votes received from each country (key = country code) |

---


*(In junior, `Score` doesn't have the per-country `votes` breakdown like in senior.)*

### Images folder (`images/`)
It contains the logo image files for each contest edition (senior and junior). Files are organized by edition and then by year.

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
