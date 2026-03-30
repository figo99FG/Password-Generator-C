# Password-Generator-C# Random Password Generator — C

A secure, multi-wordlist password generator written in C. Combines randomly selected words of varying lengths to build passwords, using millisecond-precision timestamps for entropy. Includes robust file handling with `fseek`/`ftell` and strict input validation.

---

## 🔐 Features

- Combines words from multiple dictionary files (3–7 letter words) for memorable yet strong passwords
- Millisecond-timestamp seeding (`ftime`) for true randomness between calls
- Microsecond sleep (`usleep`) between random calls to prevent seed collision
- File handling via `fseek`/`ftell` for efficient random line access
- Graceful error handling for missing or corrupt dictionary files
- Cross-platform compatible (Linux / macOS)

---

## 🛠️ Tech Stack

- **Language**: C (C99)
- **Libraries**: `stdio.h`, `stdlib.h`, `string.h`, `time.h`, `unistd.h`, `sys/timeb.h`, `sys/time.h`

---

## 📁 Project Structure

```
password-generator/
├── generator.c     # Core generator — readFile(), randNumber(), main()
├── 3               # 3-letter word list
├── 4               # 4-letter word list
├── 5               # 5-letter word list
├── 6               # 6-letter word list
├── 7               # 7-letter word list (dic4–dic7 variants)
└── README.md
```

Dictionary files are named by word length (e.g. file `5` contains 5-letter words). The generator reads from these using the `readFile()` function.

---

## ⚙️ How to Build

```bash
gcc -o generator generator.c
```

---

## 🚀 How to Run

```bash
./generator
```

---

## 🔧 Core Functions

### `readFile(char path[49], int wordLength, int rLine)`

Opens the dictionary file at `path/wordLength` and returns the word at line `rLine`.

```c
// Get word from line 42 of the 5-letter dictionary
char word[500];
strcpy(word, readFile("/path/to/dicts", 5, 42));
```

### `randNumber(int limit)`

Returns a random integer between 0 and `limit`. Seeds from millisecond timestamp and sleeps 98,765 µs between calls to ensure unique seeds.

```c
int r = randNumber(500);  // random number 0–499
```

---

## 🧠 How It Works

```
1. Choose word length randomly (e.g. 4, 6, 3...)
2. Open matching dictionary file
3. Count lines to get word pool size
4. Pick random line number using timestamp-seeded RNG
5. Read word at that line using fseek/ftell
6. Repeat and concatenate words → password
```

The use of `fseek`/`ftell` means the generator reads only the needed line — no full file load into memory.

---

## 📚 Concepts Demonstrated

- File I/O with `fopen`, `fgets`, `fseek`, `ftell`, `fclose`
- Timestamp-based entropy with `ftime` and `gettimeofday`
- String handling with `strncpy`, `memset`, `strlen`
- Memory-safe patterns with static buffers and bounds checking
- Modular C design with reusable utility functions

---

## 👤 Author

**Figo Figo** — BSc Networking & Security, Middlesex University London  
🌐 [figo.me.uk](https://figo.me.uk) · 💼 [LinkedIn](https://www.linkedin.com/in/figo-figo-1204642b2)
