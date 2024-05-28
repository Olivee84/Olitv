# Olitv
Pagina de peliculas
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Aplicación de Películas</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #141414;
            color: #fff;
            margin: 0;
            padding: 0;
        }
        .navbar {
            background-color: #000;
            padding: 10px 20px;
            display: flex;
            align-items: center;
            justify-content: space-between;
        }
        .navbar h1 {
            margin: 0;
            font-size: 24px;
        }
        .navbar ul {
            list-style: none;
            display: flex;
            margin: 0;
            padding: 0;
        }
        .navbar ul li {
            margin-left: 20px;
        }
        .navbar ul li a {
            color: #fff;
            text-decoration: none;
        }
        .container {
            padding: 20px;
        }
        .carousel {
            display: flex;
            flex-wrap: wrap;
            margin-bottom: 20px;
        }
        .carousel-row {
            display: flex;
            flex-wrap: nowrap;
            overflow-x: auto;
        }
        .carousel::-webkit-scrollbar {
            display: none;
        }
        .movie-card {
            background: #222;
            border-radius: 5px;
            margin: 10px;
            flex: 0 0 auto;
            width: 200px;
            overflow: hidden;
            position: relative;
            transition: transform 0.3s;
        }
        .movie-card img {
            width: 100%;
            display: block;
        }
        .movie-card:hover {
            transform: scale(1.1);
        }
        .movie-info {
            padding: 15px;
            text-align: center;
        }
        .movie-info h3 {
            margin: 10px 0 5px 0;
            font-size: 18px;
        }
        .movie-info p {
            font-size: 14px;
            color: #aaa;
        }
        .play-button {
            background: #e50914;
            color: #fff;
            border: none;
            padding: 10px;
            border-radius: 5px;
            cursor: pointer;
            position: absolute;
            bottom: 10px;
            left: 50%;
            transform: translateX(-50%);
        }
        .play-button:hover {
            background: #f40612;
        }
        .modal {
            display: none;
            position: fixed;
            z-index: 1;
            left: 0;
            top: 0;
            width: 100%;
            height: 100%;
            overflow: auto;
            background-color: rgba(0, 0, 0, 0.8);
        }
        .modal-content {
            position: relative;
            margin: auto;
            padding: 0;
            width: 80%;
            max-width: 800px;
            top: 50%;
            transform: translateY(-50%);
        }
        .close {
            color: #aaa;
            float: right;
            font-size: 28px;
            font-weight: bold;
            padding: 10px;
        }
        .close:hover,
        .close:focus {
            color: #fff;
            text-decoration: none;
            cursor: pointer;
        }
        video {
            width: 100%;
            height: auto;
            display: block;
        }
        .category-title {
            font-size: 24px;
            margin: 20px 0 10px 0;
        }
        .category-section {
            margin-bottom: 40px;
        }
    </style>
</head>
<body>
    <div class="navbar">
        <h1>Películas</h1>
        <ul>
            <li><a href="#" onclick="showCategory('inicio')">Inicio</a></li>
            <li><a href="#" onclick="showCategory('categorias')">Categorías</a></li>
            <li><a href="#" onclick="showCategory('miLista')">Mi lista</a></li>
        </ul>
    </div>
    <div class="container" id="main-content">
        <div id="inicio">
            <!-- Sección Inicio: Carrusel de películas -->
        </div>
        <div id="categorias" style="display: none;">
            <!-- Películas por Categorías -->
        </div>
        <div id="miLista" style="display: none;">
            <!-- Mi lista -->
        </div>
    </div>
    <div id="modal" class="modal">
        <div class="modal-content">
            <span class="close" onclick="closeModal()">&times;</span>
            <video id="videoPlayer" controls>
                <source src="" type="video/mp4">
                Tu navegador no soporta la reproducción de videos.
            </video>
        </div>
    </div>
    <script>
        const movies = [
            { id: 1, title: 'El Exorcista Creyentes', description: 'Una película de terror', videoUrl: 'http://208.88.245.85/nuevo/el.exorcista.creyentes.dual2023.mkv', category: 'Terror', imageUrl: 'https://i.ibb.co/W0syGR4/el-exorcista-creyentes.jpg' },
            { id: 2, title: 'Sonidos de Libertad', description: 'Una película dramática', videoUrl: 'http://208.88.245.85/nuevo/sonido.de.libertad.2023dual.mkv', category: 'Drama', imageUrl: 'https://i.ibb.co/GRgpMLq/sonidos-de-libertad.jpg' },
            { id: 3, title: 'Juicio al Diablo', description: 'Una película de drama', videoUrl: 'http://208.88.245.85/nuevo/juicio.al.diablo.dual.2023.mkv', category: 'Terror', imageUrl: 'https://i.postimg.cc/0yt10sBK/Juicio-al-diablo-581056175-large.jpg' },
            { id: 4, title: 'Minions: Nace un Nuevo Villano', description: 'Una película de animación', videoUrl: 'http://notabasica.com/612132/Minions.NaceUnVillano.SrRegio.Net.mp4', category: 'Animación', imageUrl: 'https://i.postimg.cc/SsLHvbKh/MV5-BNDM3-YWEw-YTMt-Nm-Y3-ZS00-Yz-Ji-LWFl-NWIt-OWFm-Nj-Y0-Yz-A4-ZDE3-Xk-Ey-Xk-Fqc-Gde-QXVy-MTA3-MDk2-NDg2-V1-QL75-UX190-CR0.jpg' },
            { id: 5, title: 'La Casa de los Abuelos', description: 'Una película de drama', videoUrl: 'http://notabasica.com/612132/The.whole.truth.2021.1080p-dual-lat-SRREGIO.NET.mkv', category: 'Drama', imageUrl: 'https://i.postimg.cc/fWmct7H1/t-Dlz-LKAm-K3-Si-Eo-Vs-MXFPGs-F9iwg.jpg' },
            { id: 6, title: 'El planeta de los simios', description: 'Una película de ciencia ficción', videoUrl: 'https://be4235.rcr32.ams02.cdn112.com/hls2/01/06182/clbkb0fvoj6v_n/master.m3u8?t=6smJU0wcP6IgvR6S9PAvxbsJQf43NJUBSclSnaS__iw&s=1716795826&e=43200&f=30913150&srv=41&asn=52362&sp=4000', category: 'ESTRENOS', imageUrl: 'https://i.postimg.cc/XvczKZkg/artworks-Pb-Vw-Bym-Rdv-Ypy4-QO-d-HNW6w-t500x500.jpg' },
            { id: 7, title: 'El telefono negro', description: 'Una película de ciencia Drama', videoUrl: 'http://notabasica.com/612132/El.Telefono.Negro.SRREGIO.NET.mkv', category: 'Terror', imageUrl: 'https://i.postimg.cc/52vsSHSR/9788418440878-n0xykt5hufcyh84i.jpg' },
            { id: 8, title: 'Lightyear', description: '', videoUrl: 'http://notabasica.com/612132/lightyear.2022.SRREGIO.mp4', category: 'Animación', imageUrl: 'https://i.postimg.cc/cJb2cgVV/Lightyear-529062623-large.jpg' },
            { id: 9, title: 'Heroico', description: 'Una película de acción', videoUrl: 'http://208.88.245.85/nuevo/heroico.latino.2023.mkv', category: 'Acción', imageUrl: 'https://i.postimg.cc/c4b4tDpf/cartel-heroico-jpg-1544685551.jpg' },
            { id: 10, title: 'Spiderman No Way Home', description: 'Una película de comedia', videoUrl: 'http://notabasica.com/612132/SpiderManNoWayHome2021.SRREGIO.NET.mkv', category: 'Acción', imageUrl: 'https://i.postimg.cc/vBNpzPN7/Spider-Man-No-Way-Home-642739124-large.jpg' },
            { id: 12, title: 'Cenicienta', description: 'Una película de ciencia ficción', videoUrl: 'https://be4235.rcr32.ams02.cdn112.com/hls2/01/06182/clbkb0fvoj6v_n/master.m3u8?t=6smJU0wcP6IgvR6S9PAvxbsJQf43NJUBSclSnaS__iw&s=1716795826&e=43200&f=30913150&srv=41&asn=52362&sp=4000', category: 'ESTRENOS', imageUrl: 'https://i.postimg.cc/0yvmRPdk/715n-GVt-OCWL-AC-UF1000-1000-QL80.jpg' },
        ];

        function showCategory(category) {
            const sections = ['inicio', 'categorias', 'miLista'];
            sections.forEach(sec => document.getElementById(sec).style.display = 'none');
            document.getElementById(category).style.display = 'block';
        }

        function displayMovies() {
            const inicioContainer = document.getElementById('inicio');
            inicioContainer.innerHTML = ''; // Limpiar contenido anterior

            const categoriasContainer = document.getElementById('categorias');
            categoriasContainer.innerHTML = ''; // Limpiar contenido anterior

            const categories = {};
            movies.forEach(movie => {
                if (!categories[movie.category]) {
                    categories[movie.category] = [];
                }
                categories[movie.category].push(movie);
            });

            const inicioCarousel = document.createElement('div');
            inicioCarousel.className = 'carousel';

            const inicioCarouselRow = document.createElement('div');
            inicioCarouselRow.className = 'carousel-row';

            let movieCount = 0;
            movies.forEach(movie => {
                if (movieCount >= 5) return; // Mostrar solo 5 películas en la sección Inicio
                movieCount++;

                const movieCard = createMovieCard(movie);
                inicioCarouselRow.appendChild(movieCard);
            });

            inicioCarousel.appendChild(inicioCarouselRow);
            inicioContainer.appendChild(inicioCarousel);

            for (const category in categories) {
                const categorySection = document.createElement('div');
                categorySection.className = 'category-section';

                const categoryTitle = document.createElement('h2');
                categoryTitle.className = 'category-title';
                categoryTitle.textContent = category;
                categorySection.appendChild(categoryTitle);

                const carousel = document.createElement('div');
                carousel.className = 'carousel';

                const carouselRow = document.createElement('div');
                carouselRow.className = 'carousel-row';

                categories[category].forEach(movie => {
                    const movieCard = createMovieCard(movie);
                    carouselRow.appendChild(movieCard);
                });

                carousel.appendChild(carouselRow);
                categorySection.appendChild(carousel);
                categoriasContainer.appendChild(categorySection);
            }
        }

        function createMovieCard(movie) {
            const movieCard = document.createElement('div');
            movieCard.className = 'movie-card';

            const movieImg = document.createElement('img');
            movieImg.src = movie.imageUrl;
            movieImg.alt = movie.title;
            movieCard.appendChild(movieImg);

            const movieInfo = document.createElement('div');
            movieInfo.className = 'movie-info';

            const movieTitle = document.createElement('h3');
            movieTitle.textContent = movie.title;
            movieInfo.appendChild(movieTitle);

            const movieDesc = document.createElement('p');
            movieDesc.textContent = movie.description;
            movieInfo.appendChild(movieDesc);

            const playButton = document.createElement('button');
            playButton.className = 'play-button';
            playButton.textContent = 'Ver';
            playButton.onclick = () => openModal(movie.videoUrl);
            movieInfo.appendChild(playButton);

            movieCard.appendChild(movieInfo);

            return movieCard;
        }

        function openModal(videoUrl) {
            const modal = document.getElementById('modal');
            const videoPlayer = document.getElementById('videoPlayer');
            videoPlayer.src = videoUrl;
            modal.style.display = 'block';
        }

        function closeModal() {
            const modal = document.getElementById('modal');
            const videoPlayer = document.getElementById('videoPlayer');
            videoPlayer.pause();
            videoPlayer.src = '';
            modal.style.display = 'none';
        }

        window.onclick = function(event) {
            const modal = document.getElementById('modal');
            if (event.target === modal) {
                closeModal();
            }
        }

        displayMovies();
    </script>
</body>
</html>
