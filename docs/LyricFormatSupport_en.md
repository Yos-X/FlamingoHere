# Lyric Formats Supported by Flamingo

Last revised: August 1, 2025


## TTML

> TTML (Timed Text Markup Language) is an XML-based timed text markup language. It is designed for cross-subtitle and subtitle delivery applications worldwide, simplifying interoperability while maintaining consistency and compatibility with other subtitle file formats.

> Its file extension is .ttml.


### Parsing Support

- Supports lyrics divided by lines and beats.
- Supports the `ttm:agent` tag.
- Supports the `ttm:role` tag, specifically `x-bg` (harmony) and `x-translation` (translation).
- Notably, it also supports marking translations via the `<translations>` tag in the TTML file header.


### Effect Support (Compared to Apple Music)

- [x] Word-by-word ascent
- [x] Duet
- [x] Multi-line simultaneous singing
- [x] Harmony (supports display above / below the main lyrics)
- [x] Glow


## LRC

> LRC (short for LyRiCs) is a computer file format that synchronizes with audio files (such as MP3, Vorbis, or MIDI). The LRC format is a text format similar to subtitle files for TV and movies.

> Its file extension is .lrc.

Over time, LRC has lacked standardized specifications from any organization, leading to various variants. Flamingo only supports the common types listed below.


### Parsing Support

#### Line-by-Line

```  
[Time]Lyrics  
[Time]Translation  
[Time]Another translation  
```  

Supports marking translations via the same timeline and multiple translations.

```  
[Time]Lyric 1  
[Time]  
[Time]Lyric 2  
```  

Supports marking countdown to singing start via empty time tags.

```  
[Time]v1: Lyrics  
```  

Supports marking singers via prefixes like `v1: `.

```  
[Time]Singer:  
[Time]Lyrics  
```  

Supports marking singers via `Singer: ` or `Singer: ` (with colon variations).

```  
[Time]Lyrics  
[bg: [Time]Harmony lyrics ]  
Note: Harmony formats other than this 
      (e.g., [bg: Harmony lyrics ]) are not supported.  
```  

Supports marking harmony via `[bg: ]`; harmony supports translations.


#### Word-by-Word

```  
[Time1]Lyric1[Time2]Lyric2[Time3]Lyric3[Time4]Lyric4[Time5]  
[Time1]Lyric1<Time2>Lyric2<Time3>Lyric3<Time4>Lyric4<Time5>  
```  

Supports word-by word-by-word timing via multiple `[]` or `<>` tags.

```  
[Time1]v1: <Time1>Lyric1<Time2>Lyric2<Time3>  
```  

Supports marking singers via prefixes like `v1: `.

```  
[Time1]Lyric1[Time2]Lyric2[Time3]  
[Time1]Translation[Time]  
                  (↑ No time restrictions here; 
                  translations will ultimately follow the main lyrics)  
```  

Supports marking translations via the same timeline (translations do not support word-by-word timing).

```  
[Time1]Lyric1[Time2]Lyric2[Time3]  
[bg: [Time1]Harmony lyric1[Time2]Harmony lyric2[Time3] ]  
```  

Supports marking harmony via `[bg: ]` (harmony supports word-by-word timing and translations).

For word-by-word lyrics, Flamingo automatically detects and adds a countdown to the start of singing.


#### Time Tags

Flamingo supports the following time tag formats:

```  
[mm:ss]  
[mm:ss.ff]  
[mm:ss.fff]  
[mm:ss:ff]  
[mm:ss:fff]  
```  

Note: "mm" and "ss" are for illustrative purposes only; actual support extends to any length of time tags.


## Other Notes

#### Lyric Formatting

To ensure clean and neat display, Flamingo's `YosLyricView` includes a built-in lyric formatting function. It performs operations on lyric text including but not limited to: **merging spaces, converting full-width / half-width symbols, and adding spacing between CJK characters and other languages**. This may result in slight visual differences between the displayed lyrics and the original file content, so please note this.