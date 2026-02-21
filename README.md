# stag-planner[index.html](https://github.com/user-attachments/files/25460210/index.html)
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Edinburgh Stag Planner</title>
  <style>
    body { font-family: Arial, sans-serif; margin: 0; background:#f5f7fb; }
    header { background:#111827; color:white; padding:16px; text-align:center; }
    nav { display:flex; gap:12px; justify-content:center; background:#1f2937; padding:10px; flex-wrap:wrap; }
    nav button { background:#374151; color:white; border:none; padding:10px 16px; border-radius:8px; cursor:pointer; }
    nav button.active { background:#2563eb; }
    .container { padding:20px; max-width:1100px; margin:auto; }
    .grid { display:grid; grid-template-columns:repeat(auto-fit,minmax(260px,1fr)); gap:20px; }
    .card { background:white; border-radius:14px; box-shadow:0 6px 18px rgba(0,0,0,0.08); overflow:hidden; display:flex; flex-direction:column; }
    .card img { width:100%; height:170px; object-fit:cover; }
    .carousel-controls { display:flex; justify-content:space-between; padding:6px; }
    .carousel-controls button { border:none; background:#e5e7eb; border-radius:6px; padding:4px 8px; cursor:pointer; }
    .card-content { padding:14px; flex:1; }
    .price { font-weight:bold; color:#047857; margin:6px 0; }
    .vote { display:flex; justify-content:space-between; align-items:center; margin-top:10px; }
    .vote button { background:#2563eb; color:white; border:none; padding:8px 12px; border-radius:8px; cursor:pointer; }
    .page { display:none; }
    .page.active { display:block; }
    footer { text-align:center; padding:20px; color:#6b7280; }
  </style>
</head>
<body>

<header>
  <h1>🍻 Edinburgh Stag Do Planner</h1>
  <p>Vote for where we stay and what we do</p>
</header>

<nav>
  <button onclick="showPage('home')" class="active">Home</button>
  <button onclick="showPage('accommodation')">Accommodation</button>
  <button onclick="showPage('activities')">Activities</button>
  <button onclick="showPage('results')">Results</button>
</nav>

<div class="container">

  <!-- HOME -->
  <div id="home" class="page active">
    <h2>Weekend Overview</h2>
    <p><strong>Location:</strong> Edinburgh</p>
    <p><strong>Dates:</strong> Friday May 15th – Sunday May 17th 2026</p>
    <p><strong>Group Size:</strong> ~8 people (sleeping for 7)</p>
    <p>Vote on the options below — results update live.</p>
  </div>

  <!-- ACCOMMODATION -->
  <div id="accommodation" class="page">
    <h2>🏠 Accommodation Options</h2>
    <div class="grid" id="accommodationList"></div>
  </div>

  <!-- ACTIVITIES -->
  <div id="activities" class="page">
    <h2>🎯 Activity Ideas</h2>
    <div class="grid" id="activityList"></div>
  </div>

  <!-- RESULTS -->
  <div id="results" class="page">
    <h2>📊 Current Votes</h2>
    <div id="resultsContent"></div>
  </div>

</div>

<footer>
  Share this link with the group and get voting.
</footer>

<script>
// ---------- DATA (EASY TO EDIT) ----------
// NOTE: To use REAL Airbnb photos reliably, download 3–6 images from each listing
// and place them in a folder called "images" next to this file.
// Example structure:
// stag-site/
//   index.html
//   images/
//     apt1-1.jpg
//     apt1-2.jpg
//     apt2-1.jpg
// Then update the paths below.

const accommodation = [
  {
    id: 'apt1',
    name: '3 Bed Garden Flat (Hot Tub)',
    price: 100,
    details: 'Sleeps 7 · 3 bedrooms · Hot tub · Near airport & city centre',
    images: [
      'jacuzzi1.avif',
      'jacuzzi2.avif',
      'jacuzzi3.avif',
      'jacuzzi4.avif',
      'jacuzzi5.avif',
      'jacuzzi6.avif'
    ],
    link: 'https://www.airbnb.co.uk/rooms/53499305'
  },
  {
    id: 'apt2',
    name: 'Castle View Group Apartment',
    price: 90,
    details: 'Sleeps up to 10 · 5 bedrooms · Roof terrace · Castle views',
    images: [
      'castle1.avif',
      'castle2.avif',
      'castle3.avif',
      'castle4.avif',
      'castle5.avif',
      'castle6.avif'
    ],
    link: 'https://www.airbnb.co.uk/rooms/1573071585601841424'
  },
  {
    id: 'apt3',
    name: 'Modern 4 Bed Home (Tranent)',
    price: 95,
    details: 'Sleeps 7 · 4 bedrooms · Modern house · Tranent',
    images: [
      'tranent1.avif',
      'tranent2.avif',
      'tranent3.avif',
      'tranent4.avif',
      'tranent5.avif',
      'tranent6.avif'
    ],
    link: 'https://www.airbnb.co.uk/rooms/1523279684303427924'
  },
  {
    id: 'apt4',
    name: 'Spacious Edinburgh Group Apartment',
    price: 95,
    details: 'Sleeps 7 · Entire apartment · Good group layout · Castle views',
    images: [
      'spacious1.avif',
      'spacious2.avif',
      'spacious3.avif',
      'spacious4.avif',
      'spacious5.avif',
      'spacious6.avif'
    ],
    link: 'https://www.airbnb.co.uk/rooms/904112976466764917'
  },
  {
    id: 'apt5',
    name: 'Luxury New Town 4-Bed Apartment',
    price: 110,
    details: 'Sleeps 7–8 · 4 bedrooms · Central New Town · City views',
    images: [
      'newtown1.avif',
      'newtown2.avif',
      'newtown3.avif',
      'newtown4.avif',
      'newtown5.avif',
      'newtown6.avif'
    ],
    link: 'https://www.airbnb.co.uk/rooms/1366173592852919210'
  }
];

const activities = [
  {
    id: 'act1',
    name: 'Whisky Tasting',
    price: 35,
    details: 'Classic Edinburgh experience · ~2 hrs',
    img: 'https://images.unsplash.com/photo-1514361892635-e9550b6c1b6a'
  },
  {
    id: 'act2',
    name: 'Go Karting',
    price: 45,
    details: 'Competitive racing · ~1.5 hrs',
    img: 'https://images.unsplash.com/photo-1517649763962-0c623066013b'
  },
  {
    id: 'act3',
    name: 'Bar Crawl',
    price: 25,
    details: 'Guided nightlife · Evening',
    img: 'https://images.unsplash.com/photo-1516455590571-18256e5bb9ff'
  },
  {
    id: 'act4',
    name: 'Highland Games',
    price: 55,
    details: 'Outdoor team events · ~3 hrs',
    img: 'https://images.unsplash.com/photo-1508609349937-5ec4ae374ebf'
  }
];

// ---------- VOTING ----------
const votes = JSON.parse(localStorage.getItem('stagVotes') || '{}');

function vote(id) {
  votes[id] = (votes[id] || 0) + 1;
  localStorage.setItem('stagVotes', JSON.stringify(votes));
  renderAll();
}

function getVotes(id) {
  return votes[id] || 0;
}

// ---------- RENDER ----------
function createCard(item) {
  const imageHtml = item.images ? `
    <div>
      <img id="img-${item.id}" src="${item.images[0]}" alt="${item.name}">
      <div class="carousel-controls">
        <button onclick="prevImage('${item.id}')">◀</button>
        <button onclick="nextImage('${item.id}')">▶</button>
      </div>
    </div>
  ` : `<img src="${item.img}" alt="${item.name}">`;

  return `
    <div class="card">
      ${imageHtml}
      <div class="card-content">
        <h3>${item.link ? `<a href="${item.link}" target="_blank" style="text-decoration:none;color:#111827">${item.name}</a>` : item.name}</h3>
        <div>${item.details}</div>
        <div class="price">~£${item.price} pp</div>
        ${item.link ? `<div style="margin:8px 0"><a href="${item.link}" target="_blank" style="color:#2563eb;font-weight:bold;text-decoration:none">View Listing →</a></div>` : ''}
        <div class="vote">
          <span>Votes: <strong>${getVotes(item.id)}</strong></span>
          <button onclick="vote('${item.id}')">Vote 👍</button>
        </div>
      </div>
    </div>
  `;
}

// ---------- IMAGE CAROUSEL STATE ----------
const imageIndex = {};

function nextImage(id){
  const item = accommodation.find(a=>a.id===id);
  if(!item || !item.images) return;
  imageIndex[id] = ((imageIndex[id]||0)+1) % item.images.length;
  document.getElementById(`img-${id}`).src = item.images[imageIndex[id]];
}

function prevImage(id){
  const item = accommodation.find(a=>a.id===id);
  if(!item || !item.images) return;
  imageIndex[id] = ((imageIndex[id]||0)-1+item.images.length) % item.images.length;
  document.getElementById(`img-${id}`).src = item.images[imageIndex[id]];
}

function renderAll(){
  document.getElementById('accommodationList').innerHTML = accommodation.map(createCard).join('');
  document.getElementById('activityList').innerHTML = activities.map(createCard).join('');

  const allItems = [...accommodation, ...activities]
    .map(i => ({ name: i.name, votes: getVotes(i.id) }))
    .sort((a,b)=>b.votes-a.votes);

  document.getElementById('resultsContent').innerHTML = allItems
    .map(i => `<p><strong>${i.name}</strong>: ${i.votes} votes</p>`)
    .join('');
}

// ---------- NAVIGATION ----------
function showPage(pageId) {
  document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
  document.getElementById(pageId).classList.add('active');

  document.querySelectorAll('nav button').forEach(b => b.classList.remove('active'));
  event.target.classList.add('active');
}

// Initial render
renderAll();

</script>

</body>
</html>
