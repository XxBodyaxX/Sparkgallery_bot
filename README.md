<script>
    const tg = window.Telegram.WebApp;
    tg.ready();
    tg.expand();

    const MAX_FILE_SIZE = 100 * 1024 * 1024;
    const MAX_STORAGE = 500 * 1024 * 1024;
    const ADMIN_ID = 'YOUR_TELEGRAM_ID'; // Замени на свой ID

    let userData = {
        id: null,
        firstName: 'Пользователь',
        username: '@demo',
        isAdmin: false,
        level: 1,
        exp: 0,
        posts: [],
        likes: 0,
        favorites: [],
        storageUsed: 0,
        warnings: 0,
        banned: false
    };

    let posts = [];
    let selectedFile = null;

    function init() {
        if (!tg.initData || !tg.initDataUnsafe || !tg.initDataUnsafe.user) {
            alert("Ошибка авторизации. Пожалуйста, откройте мини-приложение через Telegram.");
            return;
        }

        const user = tg.initDataUnsafe.user;
        userData.id = user.id;
        userData.firstName = user.first_name || 'Пользователь';
        userData.username = user.username ? `@${user.username}` : '@user';
        userData.isAdmin = user.id.toString() === ADMIN_ID;

        document.getElementById('profileName').textContent = userData.firstName;
        document.getElementById('profileUsername').textContent = userData.username;
        document.getElementById('profileAvatar').textContent = userData.firstName.charAt(0).toUpperCase();

        if (userData.isAdmin) {
            document.getElementById('adminMusic').style.display = 'block';
        }

        updateUI();
    }

    function updateUI() {
        document.getElementById('postsCount').textContent = userData.posts.length;
        document.getElementById('likesCount').textContent = userData.likes;
        document.getElementById('favoritesCount').textContent = userData.favorites.length;

        const storagePercent = (userData.storageUsed / MAX_STORAGE) * 100;
        document.getElementById('storageText').textContent = `${(userData.storageUsed / 1024 / 1024).toFixed(1)} МБ / 500 МБ`;
        document.getElementById('storageFill').style.width = `${storagePercent}%`;

        const expPercent = (userData.exp / 100) * 100;
        document.getElementById('currentExp').textContent = userData.exp;
        document.getElementById('neededExp').textContent = 100;
        document.getElementById('levelFill').style.width = `${expPercent}%`;
        document.getElementById('userLevel').textContent = userData.level;
    }

    function closeWelcome() {
        document.getElementById('welcomeScreen').classList.add('hidden');
    }

    function switchPage(page) {
        document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
        document.querySelectorAll('.nav-item').forEach(n => n.classList.remove('active'));

        const pages = {
            'profile': [document.getElementById('profilePage'), 0],
            'gallery': [document.getElementById('galleryPage'), 1],
            'favorites': [document.getElementById('favoritesPage'), 2],
            'settings': [document.getElementById('settingsPage'), 3]
        };

        if (pages[page]) {
            pages[page][0].classList.add('active');
            document.querySelectorAll('.nav-item')[pages[page][1]].classList.add('active');
        }
    }

    function openCreateModal() {
        document.getElementById('createModal').classList.add('active');
    }

    function closeCreateModal() {
        document.getElementById('createModal').classList.remove('active');
    }

    function toggleMusic() {
        const toggle = document.getElementById('musicToggle');
        const audio = document.getElementById('bgMusic');
        toggle.classList.toggle('active');

        if (toggle.classList.contains('active')) {
            audio.play();
        } else {
            audio.pause();
        }
    }

    function changeTheme(theme) {
        document.querySelectorAll('.theme-btn').forEach(btn => btn.classList.remove('active'));
        const btn = Array.from(document.querySelectorAll('.theme-btn')).find(b => b.textContent.includes(theme));
        if (btn) btn.classList.add('active');
        // Здесь можно добавить смену CSS переменных или классов
    }

    function filterPosts() {
        const filter = document.getElementById('dateFilter').value;
        // Здесь можно реализовать фильтрацию массива posts по дате
        console.log("Фильтр:", filter);
    }

    init();
</script>
