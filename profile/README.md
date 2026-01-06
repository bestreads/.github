# Bestreads

Ziel ist es, eine echte Alternative zu Goodreads zu entwickeln.
Aktuell leidet Goodreads unter einer unübersichtlichen Oberfläche, einem unvollständigen Buchindex – und die Performance ist schlicht katastrophal.

Als großes Feature kommen grundlegende Social-Media-Funktionen hinzu.
Aktivitäten wie Lesefortschritte oder Statusupdates sollen mit anderen Nutzerinnen und Nutzern geteilt werden können.


| Sprint | Zeitraum             |
| ------ | -------------------- |
| 1      | 24.11 – 05.12        |
| 2      | 08.12 – 19.12        | 
| 3      | 22.12 – 02.01  Projektpause      |
| 4      | 05.01 – 16.01 <-- Derzeit hier        |
| 5      | 19.01 – 30.01        |
| 6      | 02.02 – 13.02        Präsentation|
| 7      | 16.02 – 20.02 (kurz) Bericht|

Hochschulphase: 12.01 - 6.02




# Endpunkt Vorschläge/Wünsche 😎🥰
## Buch JSON Format:
{
    ISBN: "978-0441013593", // Braucht das Frontend eig net aber yok
    title: "Dune",
    author: "Frank Herbert",
    coverurl: "https://m.media-amazon.com/images/I/71oSHCZABCL._SY466_.jpg", // Muss noch verändert werden
    ratingavg: 4.2,
    description: "Arrakis ist eine tödliche Wüstenwelt und der einzige Fundort der Droge 'Spice', die das Reisen zwischen den Sternen ermöglicht. Als seine Familie verraten wird, beginnt für Paul Atreides ein Kampf, der das Schicksal des gesamten Universums verändern wird.",
    releasedate: 1965,
    genre: "Science Fiction",
    userBook: {
      state: "read",
      rating: 4,
    },
  },


## Userdata/stats von beliebigem User mit userId bekommen:
  const userStats = {
    userId: 123,
    username: "test123",
    profilePictureURL: "...",
    accountCreatedAtYear: 2025,
    booksInLibrary: 12,
    posts: 34,
    follower: 5678,
    following: 91011,
  }

## Folgestatus prüfen: userId1 folgt userId2? -> true/false

## Folgen: userId1 folgt jetzt userId2
