<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>BookBuddy - Your Book Recommendation Guide</title>
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <style>
    body {
      font-family: 'Segoe UI', sans-serif;
      margin: 0;
      padding: 0;
      background: #000; /* Full black background */
      color: #fff;
    }

    header {
      background: #000;
      padding: 1rem;
      text-align: center;
    }

    .logo-img {
      height: 120px;
    }

    .book-grid {
      display: grid;
      grid-template-columns: repeat(auto-fill, minmax(150px, 1fr));
      gap: 1rem;
      padding: 2rem;
      max-width: 1200px;
      margin: auto;
    }

    .book-card {
      background: #1a1a1a;
      padding: 1rem;
      border-radius: 8px;
      box-shadow: 0 2px 8px rgba(255, 255, 255, 0.1);
      text-align: center;
      cursor: pointer;
      transition: transform 0.2s;
      color: #fff;
    }

    .book-card:hover {
      transform: translateY(-5px);
    }

    .book-card img {
      width: 100%;
      height: 200px;
      object-fit: cover;
      border-radius: 5px;
    }

    .book-card h3 {
      font-size: 1rem;
      margin: 0.5rem 0 0;
    }

    /* Modal */
    .modal {
      display: none;
      position: fixed;
      z-index: 999;
      left: 0;
      top: 0;
      width: 100%;
      height: 100%;
      overflow: auto;
      background-color: rgba(0,0,0,0.8);
    }

    .modal-content {
      background-color: #222;
      margin: 5% auto;
      padding: 2rem;
      border: 1px solid #555;
      width: 80%;
      max-width: 600px;
      border-radius: 10px;
      position: relative;
      color: #fff;
    }

    .modal-content img {
      max-width: 200px;
      float: left;
      margin-right: 1rem;
      border-radius: 6px;
    }

    .modal-content h2 {
      margin-top: 0;
    }

    .close {
      color: #fff;
      position: absolute;
      right: 15px;
      top: 10px;
      font-size: 28px;
      font-weight: bold;
      cursor: pointer;
    }

    .close:hover {
      color: #FFD700;
    }

    @media (max-width: 600px) {
      .modal-content {
        padding: 1rem;
      }

      .modal-content img {
        float: none;
        display: block;
        margin: 0 auto 1rem;
      }
    }
  </style>
</head>
<body>
  <!-- Header with image logo -->
  <header>
    <img src="book buddy.png" alt="BookBuddy Logo" class="logo-img">
    <H2> TOP 55 BEST BOOKS FROM EVERY GENRE</H2>
  </header>

  <!-- Book Grid -->
  <div class="book-grid" id="bookGrid"></div>

  <!-- Modal for Book Details -->
  <div id="bookModal" class="modal">
    <div class="modal-content">
      <span class="close" onclick="closeModal()">&times;</span>
      <img id="modalImage" src="" alt="Book Cover">
      <h2 id="modalTitle"></h2>
      <h4 id="modalAuthor"></h4>
      <p id="modalDescription"></p>
    </div>
  </div>

  <!-- Book Data + Script -->
  <script>
    
    const books = [
      {
        title: "To Kill a Mockingbird",
        author: "Harper Lee",
        image: "https://covers.openlibrary.org/b/id/8228691-L.jpg",
        description: "In Maycomb, Alabama, during the 1930s, Scout Finch and her brother Jem learn about prejudice and injustice while their father, Atticus, defends a black man, Tom Robinson, wrongly accused of assaulting a white woman. Scout, Jem, and their friend Dill also become fascinated by their reclusive neighbor, Boo Radley. The trial exposes the racism of the town, and despite Tom's innocence, the jury convicts him. Ultimately, Bob Ewell, the accuser, attacks the children, and Boo Radley intervenes, saving them. The story explores themes of empathy, social inequality, and the importance of standing up for what's right. "
      },
      {
        title: "The Fault in Our Stars",
        author: "John Green",
        image: "https://covers.openlibrary.org/b/id/7275460-L.jpg",
        description: "The Fault in our stars is a young adult novel by John Green about Hazel, a 16-year-old with cancer, who meets Augustus at a support group. They fall in love, navigating the complexities of life, loss, and mortality. The story explores their relationship, their individual struggles with cancer, and their journey to find meaning in a world that often feels unfair. Ultimately, it's a poignant tale about love, loss, and the importance of living in the face of death. "
      },
      {
        title: "Wonder",
        author: "R.J. Palacio",
        image: "https://covers.openlibrary.org/b/id/7269873-L.jpg",
        description: "Wonder can refer to a feeling of amazement, awe, or admiration, often at something extraordinary or novel. It can also be used to describe something remarkable or impressive. In literature, Wonder is the title of a popular book by R.J. Palacio, which tells the story of a boy with facial differences entering mainstream school. The book explores themes of kindness, acceptance, and empathy. Science is also described as a wonder, for its many advancements in medicine, technology, and communication."
      },
      {
        title: "A Man Called Ove",
        author: "Fredrik Backman",
        image: "https://covers.openlibrary.org/b/id/8378691-L.jpg",
        description: "A Man Called Ove tells the story of Ove, a grumpy, retired man whose life is disrupted by a lively young family moving in next door. Initially, he's set in his ways, filled with bitterness after losing his wife, Sonja. He tries to maintain his routine and even plans his own death, but the new neighbors, particularly Parvaneh, begin to chip away at his hardened exterior. Through their interactions and shared experiences, Ove rediscovers the value of human connection and finds a new purpose, learning to open his heart again. "
      },
      {
        title: "Eleanor Oliphant Is Completely Fine",
        author: "Gail Honeyman",
        image: "https://covers.openlibrary.org/b/id/8379576-L.jpg",
        description: "Eleanor Oliphant Is Completely Fine by Gail Honeyman is a touching novel about a socially isolated woman who lives a life of strict routine and solitude. Eleanor struggles with social skills and emotional trauma from a tragic past, believing she is “completely fine” on her own. Her world begins to shift when she unexpectedly forms a friendship with Raymond, the kind IT guy from her office. Through small acts of kindness and human connection, Eleanor slowly begins to heal, confront her painful memories, and discover the value of vulnerability, friendship, and self-acceptance. It’s a story of quiet transformation and hope."
      },
     {
  title: "Dishonest Woman",
  author: "by Jessica Steele",
  image: "https://covers.openlibrary.org/b/id/6490176-M.jpg",
  description: "Dishonest Woman by Jessica Steele is a classic romantic tale wrapped in misunderstanding and pride. Tessa Marsden, a spirited young woman, finds herself wrongly accused of being deceitful by the brooding and powerful Ross Falconer. Their initial clashes are filled with tension, mistrust, and undeniable attraction. As secrets unravel and truths emerge, Ross begins to question his assumptions while Tessa fights to maintain her dignity and independence. Set against a backdrop of emotional turmoil and tender moments, Dishonest Woman explores how love can blossom even amidst false judgments, showing that the heart often knows more than appearances reveal."
},
{
    title: "The Mindf*ck Series",
    author: "by S.T. Abby",
    image: "https://covers.openlibrary.org/b/id/12552946-M.jpg",
    description: "The Mindf*ck Series by S.T. Abby is a dark, gripping romantic thriller told over five explosive books. It follows Lana Myers, a brilliant and deadly serial killer seeking revenge for the horrors of her past. As she methodically hunts those who destroyed her life, she unexpectedly falls for Logan Bennett — an FBI profiler hunting a killer he doesn’t realize is Lana. Tense, emotional, and morally complex, the series explores justice, trauma, and love in impossible circumstances. Twisted yet tender, *The Mindf*ck Series\* keeps readers on edge with shocking twists, fierce passion, and a heroine like no other."
  },
  {
  title: "The Spanish Love Deception",
  author: "Elena Armas",
  image: "https://covers.openlibrary.org/b/id/12668257-M.jpg",
  description: "A slow-burn, fake-dating romance with sharp banter and strong chemistry. Catalina needs a date for her sister’s wedding in Spain and ends up bringing her infuriating co-worker Aaron. What starts as a ruse soon blurs into something real, testing Catalina’s guarded heart. Packed with tension, cultural charm, and unexpected vulnerability, The Spanish Love Deception is a feel-good enemies-to-lovers journey."
},
{
  title: "The Hating Game",
  author: "Sally Thorne",
  image: "https://covers.openlibrary.org/b/id/8374981-M.jpg",
  description: "Lucy and Joshua are rivals at work, locked in a daily game of one-upmanship. But when a promotion comes between them, their mutual hatred sparks surprising attraction. With witty dialogue and slow-burning intensity, The Hating Game is a smart, steamy romantic comedy that proves the line between love and hate is dangerously thin."
},
{
  title: "It Ends with Us",
  author: "Colleen Hoover",
  image: "https://covers.openlibrary.org/b/id/11150134-M.jpg",
  description: "Lily’s love story with charming neurosurgeon Ryle appears perfect — until painful truths surface. As her past and present collide, she must make difficult choices. A powerful, emotional novel about love, resilience, and breaking cycles, It Ends with Us is a heart-wrenching exploration of strength in vulnerability."
},
{
  title: "Beautiful Disaster",
  author: "Jamie McGuire",
  image: "https://covers.openlibrary.org/b/id/8101340-M.jpg",
  description: "Good girl Abby tries to resist the reckless charm of underground fighter Travis Maddox. But their chemistry is magnetic, and their relationship soon turns chaotic. Beautiful Disaster is an intense, passionate tale of love, danger, and emotional growth, where two broken people find something worth fighting for."
},
{
  title: "The Wall of Winnipeg and Me",
  author: "Mariana Zapata",
  image: "https://covers.openlibrary.org/b/id/9251284-M.jpg",
  description: "Vanessa quits her job as assistant to cold, demanding football star Aiden Graves — only for him to ask her to marry him... for a green card. What begins as a convenient arrangement slowly builds into trust, friendship, and unexpected love. A masterclass in slow-burn romance, The Wall of Winnipeg and Me is heartfelt and satisfying."
},
{
  title: "November 9",
  author: "Colleen Hoover",
  image: "https://covers.openlibrary.org/b/id/11226178-M.jpg",
  description: "Fallon meets aspiring novelist Ben the day before her cross-country move. They agree to meet on the same date every year — November 9 — and document their connection. But as the years pass, hidden truths threaten their storybook romance. Emotional and full of twists, November 9 explores fate, forgiveness, and how timing can change everything."
},
{
  title: "Archer's Voice",
  author: "Mia Sheridan",
  image: "https://covers.openlibrary.org/b/id/8821645-M.jpg",
  description: "Bree escapes trauma by moving to a small town, where she meets Archer — a silent, mysterious man with a painful past. As they form a deep bond, Bree helps Archer find his voice in more ways than one. A beautiful story of healing and unconditional love, Archer's Voice is both tender and unforgettable."
},
{
  title: "Ugly Love",
  author: "Colleen Hoover",
  image: "https://covers.openlibrary.org/b/id/9284242-M.jpg",
  description: "Tate and Miles share an intense physical connection, but Miles refuses to open his heart. As past scars unfold, Tate must decide if love without promises is enough. Dark, passionate, and emotionally charged, Ugly Love delves into grief, longing, and the hope of redemption through love."
},
{
  title: "Wait for It",
  author: "Mariana Zapata",
  image: "https://covers.openlibrary.org/b/id/9349058-M.jpg",
  description: "Diana, a single guardian to her nephews, is strong and independent — until her grumpy neighbor Dallas challenges her in unexpected ways. A slow-burn romance full of heartfelt family moments and real emotional growth, Wait for It delivers quiet intensity and deep connection."
},
{
  title: "On the Island",
  author: "Tracey Garvis Graves",
  image: "https://covers.openlibrary.org/b/id/7858795-M.jpg",
  description: "Anna, a teacher, and her student T.J. are stranded on a remote island after a plane crash. As years pass in isolation, their bond turns into love. A story of survival, emotional strength, and unexpected romance, On the Island explores love’s ability to thrive in even the most unlikely places."
},{
  title: "Before We Were Strangers",
  author: "Renée Carlino",
  image: "https://covers.openlibrary.org/b/id/8238585-M.jpg",
  description: "Grace and Matt were once inseparable college sweethearts, torn apart by time and miscommunication. Fifteen years later, a missed connection ad sparks a reunion that reopens old wounds and rekindles old flames. Before We Were Strangers is a nostalgic, tender second-chance romance that explores what it means to love, lose, and rediscover the one who got away."
},
{
  title: "Confess",
  author: "Colleen Hoover",
  image: "https://covers.openlibrary.org/b/id/9284244-M.jpg",
  description: "Auburn walks into an art studio looking for a job, but instead finds Owen — a talented artist who paints based on anonymous confessions. Their connection is immediate, but both hide painful secrets that could destroy their fragile trust. Confess is a story about truth, sacrifice, and the risks we take for love."
},
{
  title: "Reminders of Him",
  author: "Colleen Hoover",
  image: "https://covers.openlibrary.org/b/id/12710638-M.jpg",
  description: "After serving five years in prison, Kenna returns home to reconnect with her daughter — but everyone wants her gone. Except for Ledger, a local bar owner with ties to Kenna’s past. Torn between loyalty and love, Ledger and Kenna must navigate pain, redemption, and healing. Reminders of Him is raw, emotional, and full of second chances."
},
{
  title: "The Simple Wild",
  author: "K.A. Tucker",
  image: "https://covers.openlibrary.org/b/id/9252082-M.jpg",
  description: "City girl Calla journeys to Alaska to reconnect with her estranged, terminally ill father. There, she clashes with rugged pilot Jonah — who sees her as spoiled and fragile. But as they face life’s harsh realities, unexpected sparks fly. The Simple Wild is a moving story of family, self-discovery, and opposites attracting in the unlikeliest place."
},
{
  title: "Behind Closed Doors",
  author: "B.A. Paris",
  image: "https://covers.openlibrary.org/b/id/8072064-M.jpg",
  description: "Jack and Grace appear to be the perfect couple, but behind closed doors, nothing is as it seems. This psychological romance-thriller delves into control, appearances, and the terrifying dynamics of a relationship built on manipulation. Gripping and chilling, Behind Closed Doors is a haunting look at love turned toxic."
},
{
  title: "The Haunting of Hill House",
  author: "Shirley Jackson",
  image: "https://covers.openlibrary.org/b/id/8228691-M.jpg",
  description: "Dr. Montague invites a group to investigate the mysterious Hill House, rumored to be haunted. Among them is Eleanor, whose fragile psyche makes her vulnerable to the house’s strange and sinister influence. With creeping dread and psychological horror, this classic explores madness, isolation, and the terror of the unknown."
},
{
  title: "Mexican Gothic",
  author: "Silvia Moreno-Garcia",
  image: "https://covers.openlibrary.org/b/id/10522658-M.jpg",
  description: "Noemí travels to a remote Mexican manor to check on her cousin, only to uncover horrifying secrets buried in the walls. A decaying house, eerie dreams, and a family with a dark past come together in this gothic horror filled with haunting atmosphere and feminist undertones."
},
{
  title: "The Silent Patient",
  author: "Alex Michaelides",
  image: "https://covers.openlibrary.org/b/id/9357097-M.jpg",
  description: "Alicia Berenson, a famous painter, shoots her husband and then never speaks another word. As a criminal psychotherapist tries to unlock her silence, shocking secrets unravel. A psychological thriller that blends horror, mystery, and an unforgettable twist, The Silent Patient keeps readers guessing until the final page."
},
{
  title: "The Only Good Indians",
  author: "Stephen Graham Jones",
  image: "https://covers.openlibrary.org/b/id/10668039-M.jpg",
  description: "Four Native American friends are haunted by a disturbing event from their past. Years later, an entity tied to ancient tradition begins stalking them. A mix of supernatural horror, social commentary, and cultural trauma, this novel is visceral, unsettling, and powerfully unique."
},
{
  title: "House of Leaves",
  author: "Mark Z. Danielewski",
  image: "https://covers.openlibrary.org/b/id/8764684-M.jpg",
  description: "When a family discovers their house is bigger on the inside than the outside, a terrifying labyrinth emerges. Told through layered narratives and unsettling formats, this postmodern horror novel explores madness, fear, and obsession — pushing the boundaries of reality and storytelling."
},{
  title: "The Shining",
  author: "Stephen King",
  image: "https://covers.openlibrary.org/b/id/8231993-M.jpg",
  description: "Jack Torrance takes a job as winter caretaker of the isolated Overlook Hotel, hoping to rebuild his life. But the hotel's malevolent presence preys on his weaknesses, pushing him toward madness. A chilling tale of descent, family tension, and supernatural horror from the master of the genre."
},
{
  title: "Bird Box",
  author: "Josh Malerman",
  image: "https://covers.openlibrary.org/b/id/8281991-M.jpg",
  description: "In a post-apocalyptic world plagued by mysterious creatures that drive people insane when seen, Malorie must guide her children to safety — blindfolded. Terrifying and tense, Bird Box is a gripping psychological survival horror novel that plays on fear of the unseen."
},
{
  title: "The Ritual",
  author: "Adam Nevill",
  image: "https://covers.openlibrary.org/b/id/8238640-M.jpg",
  description: "Four friends on a hiking trip in the Scandinavian wilderness stumble upon a deserted house and pagan symbols in the woods. What follows is a slow-burn descent into ancient terror and psychological unraveling. The Ritual is a visceral, atmospheric novel of primal fear."
},
{
  title: "The Exorcist",
  author: "William Peter Blatty",
  image: "https://covers.openlibrary.org/b/id/8107491-M.jpg",
  description: "When a young girl named Regan begins exhibiting violent and disturbing behavior, her mother seeks help from doctors and eventually a priest. What follows is a harrowing battle between good and evil. Based on true events, The Exorcist remains one of the most iconic horror novels ever written."
},
{
  title: "Something Wicked This Way Comes",
  author: "Ray Bradbury",
  image: "https://covers.openlibrary.org/b/id/8739130-M.jpg",
  description: "A dark carnival comes to a small town, offering twisted wishes with sinister consequences. Two boys find themselves fighting against the mysterious Mr. Dark. A poetic and eerie tale of growing up, temptation, and the darkness lurking beneath joy."
},
{
  title: "Pet Sematary",
  author: "Stephen King",
  image: "https://covers.openlibrary.org/b/id/8231045-M.jpg",
  description: "When Louis Creed discovers an ancient burial ground behind his new home, he learns that some things are better left dead. A tragic story of grief, resurrection, and evil, Pet Sematary is among King’s darkest and most haunting works."
},
{
  title: "My Best Friend's Exorcism",
  author: "Grady Hendrix",
  image: "https://covers.openlibrary.org/b/id/10436113-M.jpg",
  description: "Set in the 1980s, two best friends must fight a demonic possession after one of them begins acting strange. With humor, heart, and horror, this book mixes nostalgia with terrifying possession themes — a fun and emotional horror ride."
},
{
  title: "The Troop",
  author: "Nick Cutter",
  image: "https://covers.openlibrary.org/b/id/8763843-M.jpg",
  description: "A troop of boys on a camping trip face a horrifying biological threat when a mysterious, starving man stumbles into their camp. What follows is a gruesome fight for survival. The Troop is graphic, intense, and brilliantly terrifying — not for the faint of heart."
},
{
  title: "Dracula",
  author: "Bram Stoker",
  image: "https://covers.openlibrary.org/b/id/8225294-M.jpg",
  description: "A young lawyer travels to Transylvania and meets Count Dracula, unknowingly unleashing a dark force upon Victorian England. A timeless gothic horror classic that established the modern vampire myth."
},
{
  title: "Hell House",
  author: "Richard Matheson",
  image: "https://covers.openlibrary.org/b/id/8238922-M.jpg",
  description: "Four investigators enter the infamous Belasco House, the most haunted house in the world. What awaits them is pure terror. A psychological and supernatural horror novel that delivers relentless dread."
},
{
  title: "Carrion Comfort",
  author: "Dan Simmons",
  image: "https://covers.openlibrary.org/b/id/8239863-M.jpg",
  description: "Three powerful beings use psychic mind control to manipulate others into committing murder. Spanning continents and decades, this epic horror novel explores power, evil, and survival."
},
{
  title: "The Girl from the Well",
  author: "Rin Chupeco",
  image: "https://covers.openlibrary.org/b/id/9254765-M.jpg",
  description: "A vengeful ghost who kills murderers finds herself drawn to a boy possessed by an ancient evil. Inspired by Japanese folklore, this YA horror is eerie, haunting, and emotionally resonant."
},
{
  title: "Rebecca",
  author: "Daphne du Maurier",
  image: "https://covers.openlibrary.org/b/id/8347854-M.jpg",
  description: "A young woman marries a wealthy widower, only to find herself haunted by the legacy of his first wife. A gothic masterpiece filled with suspense, mystery, and dark romance."
},
{
  title: "The Turn of the Screw",
  author: "Henry James",
  image: "https://covers.openlibrary.org/b/id/8082189-M.jpg",
  description: "A governess believes her young charges are haunted by ghosts. But is it real, or her imagination? A psychological ghost story that leaves readers questioning reality."
},
{
  title: "Coraline",
  author: "Neil Gaiman",
  image: "https://covers.openlibrary.org/b/id/8238438-M.jpg",
  description: "Coraline discovers a secret door to a mirror world that hides a sinister truth. A dark fairy tale with horror undertones that captivates children and adults alike."
},
{
  title: "The Cabin at the End of the World",
  author: "Paul Tremblay",
  image: "https://covers.openlibrary.org/b/id/8946129-M.jpg",
  description: "A family vacationing in a remote cabin is held hostage by strangers who believe they must sacrifice one of their own to prevent the apocalypse. A tense, claustrophobic horror thriller."
},
{
  title: "Come Closer",
  author: "Sara Gran",
 image: "https://covers.openlibrary.org/b/id/8824826-M.jpg",
  description: "A woman’s life unravels as she slowly becomes possessed by a demon. Told in chilling first-person, this psychological horror novel explores identity and madness in terrifying ways."
},
{
  title: "We Have Always Lived in the Castle",
  author: "Shirley Jackson",
  image: "https://covers.openlibrary.org/b/id/8230811-M.jpg",
  description: "Two sisters live in isolation after a family tragedy, shunned by their town. A macabre, twisted tale of secrets, mental illness, and gothic suspense from a literary legend."
},
{
  title: "Final Girls",
  author: "Riley Sager",
  image: "https://covers.openlibrary.org/b/id/8464913-M.jpg",
  description: "Quincy survived a massacre, making her a 'Final Girl.' When another Final Girl is murdered, Quincy must confront her past. A modern slasher-inspired psychological thriller with killer twists."
},
{
  title: "The Deep",
  author: "Nick Cutter",
  image: "https://covers.openlibrary.org/b/id/8914235-M.jpg",
  description: "At the bottom of the Pacific, a research lab explores a cure for a global disease — but something ancient and evil awaits. Claustrophobic, nightmarish, and deeply unsettling."
},
{
  title: "Dead Silence",
  author: "S.A. Barnes",
  image: "https://covers.openlibrary.org/b/id/12918923-M.jpg",
  description: "A crew discovers a long-lost luxury space liner and awakens horrors that should have remained buried. Think *Titanic* meets *Event Horizon* — a sci-fi horror thrill ride."
},
{
  title: "The Fisherman",
  author: "John Langan",
  image: "https://covers.openlibrary.org/b/id/8883186-M.jpg",
  description: "Two grieving widowers find more than peace in upstate New York — they discover a dark myth that blurs reality and legend. A slow-burn cosmic horror tale filled with dread."
},
{
  title: "Horrid",
  author: "Katrina Leno",
  image: "https://covers.openlibrary.org/b/id/10465294-M.jpg",
  description: "After her father’s death, Jane and her mother move into a creepy old house with a dark history. Strange happenings and mental illness blur the lines of horror and grief."
},
{
  title: "Salem's Lot",
  author: "Stephen King",
  image: "https://covers.openlibrary.org/b/id/8231070-M.jpg",
  description: "A writer returns to his hometown only to discover that the townspeople are being turned into vampires. A modern vampire horror novel steeped in atmosphere and escalating dread."
},
{
  title: "Plain Bad Heroines",
  author: "Emily M. Danforth",
 image: "https://covers.openlibrary.org/b/id/10720357-M.jpg",
  description: "An all-girls school, a cursed book, and tragic deaths create a haunting legacy. Told across timelines, this queer horror-comedy blends gothic, satire, and supernatural chills."
},
{
  title: "The Family Plot",
  author: "Cherie Priest",
  image: "https://covers.openlibrary.org/b/id/9318756-M.jpg",
  description: "A salvage crew dismantles a historic mansion and awakens a ghostly presence. A Southern Gothic horror novel filled with haunted-house tension and family secrets."
},
{
  title: "Dark Matter",
  author: "Michelle Paver",
  image: "https://covers.openlibrary.org/b/id/8783297-M.jpg",
  description: "Set in the Arctic in 1937, a failed expedition leaves a man alone in total darkness — but he’s not truly alone. A chilling ghost story filled with isolation, dread, and psychological horror."
},
{
  title: "The House on Needless Street",
  author: "Catriona Ward",
  image: "https://covers.openlibrary.org/b/id/10771058-M.jpg",
  description: "A missing child, a disturbed man, a mysterious cat, and a woman seeking answers converge at a strange house near the woods. A mind-bending psychological horror full of twists and emotional depth."
},
{
  title: "The Changeling",
  author: "Victor LaValle",
  image: "https://covers.openlibrary.org/b/id/8372124-M.jpg",
  description: "Apollo’s perfect life unravels when his wife commits an unthinkable act and vanishes. His journey to find her leads through a surreal, nightmarish New York where myths become real. A genre-bending blend of horror, fairy tale, and social commentary on fatherhood and trauma."
}


    ];

    const bookGrid = document.getElementById("bookGrid");

    books.forEach((book) => {
      const card = document.createElement("div");
      card.className = "book-card";
      card.innerHTML = `
        <img src="${book.image}" alt="${book.title}">
        <h3>${book.title}</h3>
      `;
      card.onclick = () => showBookDetails(book);
      bookGrid.appendChild(card);
    });

    function showBookDetails(book) {
      document.getElementById("modalTitle").textContent = book.title;
      document.getElementById("modalAuthor").textContent = `by ${book.author}`;
      document.getElementById("modalDescription").textContent = book.description;
      document.getElementById("modalImage").src = book.image;
      document.getElementById("bookModal").style.display = "block";
    }

    function closeModal() {
      document.getElementById("bookModal").style.display = "none";
    }

    window.onclick = function(event) {
      const modal = document.getElementById("bookModal");
      if (event.target == modal) {
        closeModal();
      }
    }
  </script>

  <!-- Watson Assistant Chat Widget -->
  <script>
    window.watsonAssistantChatOptions = {
      integrationID: "ccb3b269-59bc-45b4-865c-792c65b39a55",
      region: "au-syd",
      serviceInstanceID: "27989855-8ba1-4532-bb48-e8c9b23bae52",
      onLoad: async (instance) => { await instance.render(); }
    };
    setTimeout(function(){
      const t = document.createElement('script');
      t.src = "https://web-chat.global.assistant.watson.appdomain.cloud/versions/" +
              (window.watsonAssistantChatOptions.clientVersion || 'latest') +
              "/WatsonAssistantChatEntry.js";
      document.head.appendChild(t);
    });
  </script>
</body>
</html>
