
<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Dashboard Admin</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <style>
        :root {
            --primary: #667eea;
            --secondary: #764ba2;
            --success: #10b981;
            --danger: #ef4444;
            --warning: #f59e0b;
            --info: #3b82f6;
            --dark: #1f2937;
            --light: #f8fafc;
            --gray: #6b7280;
            --sidebar-width: 250px;
            --header-height: 60px;
            --card-bg: #ffffff;
            --bg-color: #f1f5f9;
            --text-color: #1f2937;
            --border-color: #e5e7eb;
            --hover-color: #f3f4f6;
        }

        .dark-mode {
            --card-bg: #374151;
            --bg-color: #111827;
            --text-color: #f9fafb;
            --border-color: #4b5563;
            --hover-color: #4b5563;
        }

        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            transition: background-color 0.3s, color 0.3s;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background-color: var(--bg-color);
            color: var(--text-color);
            line-height: 1.6;
        }

        /* Layout */
        .dashboard-container {
            display: flex;
            min-height: 100vh;
        }

        /* Sidebar */
        .sidebar {
            width: var(--sidebar-width);
            background: linear-gradient(180deg, var(--primary) 0%, var(--secondary) 100%);
            color: white;
            position: fixed;
            height: 100vh;
            overflow-y: auto;
            z-index: 1000;
        }

        .logo {
            padding: 20px;
            text-align: center;
            border-bottom: 1px solid rgba(255,255,255,0.1);
        }

        .logo h2 {
            font-size: 1.3rem;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 10px;
        }

        .sidebar-nav ul {
            list-style: none;
            padding: 10px 0;
        }

        .sidebar-nav li {
            padding: 12px 20px;
            transition: background 0.3s;
            cursor: pointer;
        }

        .sidebar-nav li:hover, .sidebar-nav li.active {
            background: rgba(255,255,255,0.1);
        }

        .sidebar-nav li.active {
            border-left: 4px solid white;
        }

        .sidebar-nav a {
            color: white;
            text-decoration: none;
            display: flex;
            align-items: center;
            gap: 12px;
        }

        .sidebar-nav i {
            width: 20px;
            text-align: center;
        }

        /* Main Content */
        .main-content {
            flex: 1;
            margin-left: var(--sidebar-width);
            padding: 20px;
        }

        /* Header */
        .header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 30px;
            background: var(--card-bg);
            padding: 15px 20px;
            border-radius: 10px;
            box-shadow: 0 2px 10px rgba(0,0,0,0.05);
        }

        .header-right {
            display: flex;
            align-items: center;
            gap: 20px;
        }

        .search-box {
            position: relative;
        }

        .search-box input {
            padding: 10px 40px 10px 15px;
            border: 1px solid var(--border-color);
            border-radius: 25px;
            outline: none;
            width: 250px;
            background: var(--card-bg);
            color: var(--text-color);
        }

        .search-box i {
            position: absolute;
            right: 15px;
            top: 50%;
            transform: translateY(-50%);
            color: var(--gray);
        }

        .user-profile {
            display: flex;
            align-items: center;
            gap: 10px;
        }

        .user-profile img {
            border-radius: 50%;
            width: 40px;
            height: 40px;
        }

        .theme-toggle, .language-toggle {
            background: none;
            border: none;
            font-size: 1.2rem;
            color: var(--text-color);
            cursor: pointer;
        }

        /* Stats Cards */
        .stats-cards {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
            gap: 20px;
            margin-bottom: 30px;
        }

        .card {
            background: var(--card-bg);
            padding: 25px;
            border-radius: 10px;
            box-shadow: 0 2px 10px rgba(0,0,0,0.05);
            display: flex;
            align-items: center;
            gap: 15px;
            transition: transform 0.3s;
        }

        .card:hover {
            transform: translateY(-5px);
        }

        .card-icon {
            width: 60px;
            height: 60px;
            border-radius: 10px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 24px;
            color: white;
        }

        .card-icon.users { background: var(--success); }
        .card-icon.revenue { background: var(--info); }
        .card-icon.orders { background: var(--warning); }
        .card-icon.growth { background: var(--secondary); }

        .card-info h3 {
            font-size: 1.8rem;
            margin-bottom: 5px;
        }

        .card-info p {
            color: var(--gray);
            font-size: 0.9rem;
        }

        .card-trend {
            margin-left: auto;
            padding: 5px 10px;
            border-radius: 15px;
            font-size: 0.8rem;
            font-weight: bold;
        }

        .card-trend.up {
            background: #e8f5e8;
            color: var(--success);
        }

        .card-trend.down {
            background: #ffe8e8;
            color: var(--danger);
        }

        /* Content Grid */
        .content-grid {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 20px;
        }

        .grid-item {
            background: var(--card-bg);
            padding: 25px;
            border-radius: 10px;
            box-shadow: 0 2px 10px rgba(0,0,0,0.05);
        }

        .grid-item h3 {
            margin-bottom: 20px;
            color: var(--text-color);
            border-bottom: 2px solid var(--border-color);
            padding-bottom: 10px;
        }

        /* Recent Activity */
        .activity-item {
            display: flex;
            align-items: center;
            gap: 15px;
            padding: 15px 0;
            border-bottom: 1px solid var(--border-color);
        }

        .activity-item:last-child {
            border-bottom: none;
        }

        .activity-icon {
            width: 40px;
            height: 40px;
            background: var(--hover-color);
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            color: var(--primary);
        }

        .activity-content p {
            margin-bottom: 5px;
        }

        .activity-content span {
            color: var(--gray);
            font-size: 0.8rem;
        }

        /* Recent Users Table */
        table {
            width: 100%;
            border-collapse: collapse;
        }

        th, td {
            padding: 12px 15px;
            text-align: left;
            border-bottom: 1px solid var(--border-color);
        }

        th {
            background: var(--hover-color);
            font-weight: 600;
            color: var(--text-color);
        }

        td {
            vertical-align: middle;
        }

        .user-cell {
            display: flex;
            align-items: center;
            gap: 10px;
        }

        .user-cell img {
            border-radius: 50%;
            width: 30px;
            height: 30px;
        }

        .status {
            padding: 5px 10px;
            border-radius: 15px;
            font-size: 0.8rem;
            font-weight: bold;
        }

        .status.active {
            background: #e8f5e8;
            color: var(--success);
        }

        .status.inactive {
            background: #fff3e0;
            color: var(--warning);
        }

        .btn-edit, .btn-delete {
            background: none;
            border: none;
            cursor: pointer;
            padding: 5px;
            margin: 0 2px;
            color: var(--gray);
            transition: color 0.3s;
        }

        .btn-edit:hover {
            color: var(--info);
        }

        .btn-delete:hover {
            color: var(--danger);
        }

        /* Quick Actions */
        .quick-actions {
            display: flex;
            gap: 10px;
            margin-bottom: 20px;
            flex-wrap: wrap;
        }

        .action-btn {
            background: var(--card-bg);
            border: 1px solid var(--border-color);
            padding: 10px 15px;
            border-radius: 8px;
            display: flex;
            align-items: center;
            gap: 8px;
            cursor: pointer;
            transition: all 0.3s;
            color: var(--text-color);
        }

        .action-btn:hover {
            background: var(--primary);
            color: white;
        }

        /* Page Content */
        .page-content {
            display: none;
        }

        .page-content.active {
            display: block;
        }

        /* Modal */
        .modal {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background: rgba(0,0,0,0.5);
            align-items: center;
            justify-content: center;
            z-index: 1100;
        }

        .modal.active {
            display: flex;
        }

        .modal-content {
            background: var(--card-bg);
            border-radius: 10px;
            width: 90%;
            max-width: 500px;
            max-height: 90vh;
            overflow-y: auto;
            box-shadow: 0 10px 30px rgba(0,0,0,0.2);
        }

        .modal-header {
            padding: 20px;
            border-bottom: 1px solid var(--border-color);
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        .modal-header h3 {
            margin: 0;
            border: none;
            padding: 0;
        }

        .close-modal {
            background: none;
            border: none;
            font-size: 1.2rem;
            color: var(--gray);
            cursor: pointer;
        }

        .modal-body {
            padding: 20px;
        }

        .form-group {
            margin-bottom: 15px;
        }

        .form-group label {
            display: block;
            margin-bottom: 5px;
            font-weight: 500;
        }

        .form-control {
            width: 100%;
            padding: 10px 15px;
            border: 1px solid var(--border-color);
            border-radius: 5px;
            background: var(--card-bg);
            color: var(--text-color);
        }

        .form-actions {
            display: flex;
            justify-content: flex-end;
            gap: 10px;
            margin-top: 20px;
        }

        .btn {
            padding: 10px 20px;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            font-weight: 500;
            transition: background 0.3s;
        }

        .btn-primary {
            background: var(--primary);
            color: white;
        }

        .btn-secondary {
            background: var(--gray);
            color: white;
        }

        /* Analytics */
        .chart-container {
            position: relative;
            height: 300px;
            margin-bottom: 20px;
        }

        .analytics-filters {
            display: flex;
            gap: 15px;
            margin-bottom: 20px;
            flex-wrap: wrap;
        }

        .filter-group {
            display: flex;
            flex-direction: column;
            gap: 5px;
        }

        .filter-group label {
            font-size: 0.9rem;
            font-weight: 500;
        }

        /* Empty State */
        .empty-state {
            text-align: center;
            padding: 40px 20px;
            color: var(--gray);
        }

        .empty-state i {
            font-size: 3rem;
            margin-bottom: 15px;
            opacity: 0.5;
        }

        .empty-state p {
            margin-bottom: 10px;
        }

        /* Mobile Responsive */
        .menu-toggle {
            display: none;
            background: none;
            border: none;
            font-size: 1.2rem;
            color: var(--text-color);
        }

        @media (max-width: 768px) {
            .sidebar {
                transform: translateX(-100%);
                transition: transform 0.3s;
            }
            
            .sidebar.active {
                transform: translateX(0);
            }
            
            .main-content {
                margin-left: 0;
            }
            
            .menu-toggle {
                display: block;
            }
            
            .content-grid {
                grid-template-columns: 1fr;
            }
            
            .search-box input {
                width: 150px;
            }

            .analytics-filters {
                flex-direction: column;
            }
        }
    </style>
</head>
<body>
    <div class="dashboard-container">
        <!-- Sidebar -->
        <aside class="sidebar">
            <div class="logo">
                <h2><i class="fas fa-chart-line"></i> AdminPanel</h2>
            </div>
            <nav class="sidebar-nav">
                <ul>
                    <li class="active" data-page="dashboard"><a href="#"><i class="fas fa-home"></i> <span data-lang="dashboard">Dashboard</span></a></li>
                    <li data-page="users"><a href="#"><i class="fas fa-users"></i> <span data-lang="users">Pengguna</span></a></li>
                    <li data-page="products"><a href="#"><i class="fas fa-box"></i> <span data-lang="products">Produk</span></a></li>
                    <li data-page="analytics"><a href="#"><i class="fas fa-chart-bar"></i> <span data-lang="analytics">Analitik</span></a></li>
                    <li data-page="settings"><a href="#"><i class="fas fa-cog"></i> <span data-lang="settings">Pengaturan</span></a></li>
                    <li><a href="#"><i class="fas fa-sign-out-alt"></i> <span data-lang="logout">Keluar</span></a></li>
                </ul>
            </nav>
        </aside>

        <!-- Main Content -->
        <main class="main-content">
            <!-- Header -->
            <header class="header">
                <div class="header-left">
                    <button class="menu-toggle" id="menuToggle">
                        <i class="fas fa-bars"></i>
                    </button>
                    <h1 data-lang="dashboard">Dashboard</h1>
                </div>
                <div class="header-right">
                    <div class="search-box">
                        <input type="text" placeholder="Cari..." data-lang="searchPlaceholder" id="searchInput">
                        <i class="fas fa-search"></i>
                    </div>
                    <button class="theme-toggle" id="themeToggle">
                        <i class="fas fa-moon"></i>
                    </button>
                    <button class="language-toggle" id="languageToggle">
                        <i class="fas fa-globe"></i> <span id="languageText">ID</span>
                    </button>
                    <div class="user-profile">
                        <img src="https://ui-avatars.com/api/?name=Admin&background=667eea&color=fff" alt="Profile">
                        <span>Admin</span>
                    </div>
                </div>
            </header>

            <!-- Quick Actions -->
            <div class="quick-actions">
                <button class="action-btn" data-action="addUser">
                    <i class="fas fa-user-plus"></i>
                    <span data-lang="addUser">Tambah Pengguna</span>
                </button>
                <button class="action-btn" data-action="addProduct">
                    <i class="fas fa-box"></i>
                    <span data-lang="addProduct">Tambah Produk</span>
                </button>
                <button class="action-btn" data-action="viewReports">
                    <i class="fas fa-chart-bar"></i>
                    <span data-lang="viewReports">Lihat Laporan</span>
                </button>
                <button class="action-btn" data-action="sendNotification">
                    <i class="fas fa-bell"></i>
                    <span data-lang="sendNotification">Kirim Notifikasi</span>
                </button>
            </div>

            <!-- Dashboard Content -->
            <div id="dashboardContent" class="page-content active">
                <!-- Stats Cards -->
                <section class="stats-cards">
                    <div class="card">
                        <div class="card-icon users">
                            <i class="fas fa-users"></i>
                        </div>
                        <div class="card-info">
                            <h3 id="users-count">0</h3>
                            <p data-lang="totalUsers">Total Pengguna</p>
                        </div>
                        <div class="card-trend up">
                            +0%
                        </div>
                    </div>

                    <div class="card">
                        <div class="card-icon revenue">
                            <i class="fas fa-dollar-sign"></i>
                        </div>
                        <div class="card-info">
                            <h3 id="revenue-count">$0</h3>
                            <p data-lang="totalRevenue">Total Pendapatan</p>
                        </div>
                        <div class="card-trend up">
                            +0%
                        </div>
                    </div>

                    <div class="card">
                        <div class="card-icon orders">
                            <i class="fas fa-shopping-cart"></i>
                        </div>
                        <div class="card-info">
                            <h3 id="orders-count">0</h3>
                            <p data-lang="orders">Pesanan</p>
                        </div>
                        <div class="card-trend down">
                            -0%
                        </div>
                    </div>

                    <div class="card">
                        <div class="card-icon growth">
                            <i class="fas fa-chart-line"></i>
                        </div>
                        <div class="card-info">
                            <h3 id="growth-count">0%</h3>
                            <p data-lang="growth">Pertumbuhan</p>
                        </div>
                        <div class="card-trend up">
                            +0%
                        </div>
                    </div>
                </section>

                <!-- Charts & Tables Section -->
                <section class="content-grid">
                    <!-- Recent Activity -->
                    <div class="grid-item recent-activity">
                        <h3 data-lang="recentActivity">Aktivitas Terbaru</h3>
                        <div class="activity-list" id="activityList">
                            <div class="empty-state">
                                <i class="fas fa-inbox"></i>
                                <p>Tidak ada aktivitas terbaru</p>
                                <small>Data aktivitas akan muncul di sini</small>
                            </div>
                        </div>
                    </div>

                    <!-- Recent Users -->
                    <div class="grid-item recent-users">
                        <h3 data-lang="recentUsers">Pengguna Terbaru</h3>
                        <table>
                            <thead>
                                <tr>
                                    <th data-lang="name">Nama</th>
                                    <th data-lang="email">Email</th>
                                    <th data-lang="status">Status</th>
                                    <th data-lang="actions">Aksi</th>
                                </tr>
                            </thead>
                            <tbody id="usersTableBody">
                                <tr>
                                    <td colspan="4" style="text-align: center; padding: 40px;">
                                        <div class="empty-state">
                                            <i class="fas fa-users"></i>
                                            <p>Belum ada pengguna</p>
                                            <small>Pengguna yang ditambahkan akan muncul di sini</small>
                                        </div>
                                    </td>
                                </tr>
                            </tbody>
                        </table>
                    </div>
                </section>
            </div>

            <!-- Users Content -->
            <div id="usersContent" class="page-content">
                <div class="grid-item">
                    <div class="section-header" style="display: flex; justify-content: space-between; align-items: center; margin-bottom: 20px;">
                        <h3 data-lang="userManagement">Manajemen Pengguna</h3>
                        <button class="btn btn-primary" data-action="addUser">
                            <i class="fas fa-user-plus"></i> <span data-lang="addUser">Tambah Pengguna</span>
                        </button>
                    </div>
                    <table>
                        <thead>
                            <tr>
                                <th data-lang="name">Nama</th>
                                <th data-lang="email">Email</th>
                                <th data-lang="role">Peran</th>
                                <th data-lang="status">Status</th>
                                <th data-lang="actions">Aksi</th>
                            </tr>
                        </thead>
                        <tbody id="allUsersTableBody">
                            <tr>
                                <td colspan="5" style="text-align: center; padding: 40px;">
                                    <div class="empty-state">
                                        <i class="fas fa-users"></i>
                                        <p>Belum ada pengguna</p>
                                        <small>Klik "Tambah Pengguna" untuk menambahkan pengguna pertama</small>
                                    </div>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </div>
            </div>

            <!-- Products Content -->
            <div id="productsContent" class="page-content">
                <div class="grid-item">
                    <div class="section-header" style="display: flex; justify-content: space-between; align-items: center; margin-bottom: 20px;">
                        <h3 data-lang="productManagement">Manajemen Produk</h3>
                        <button class="btn btn-primary" data-action="addProduct">
                            <i class="fas fa-box"></i> <span data-lang="addProduct">Tambah Produk</span>
                        </button>
                    </div>
                    <table>
                        <thead>
                            <tr>
                                <th data-lang="productName">Nama Produk</th>
                                <th data-lang="category">Kategori</th>
                                <th data-lang="price">Harga</th>
                                <th data-lang="stock">Stok</th>
                                <th data-lang="status">Status</th>
                                <th data-lang="actions">Aksi</th>
                            </tr>
                        </thead>
                        <tbody id="productsTableBody">
                            <tr>
                                <td colspan="6" style="text-align: center; padding: 40px;">
                                    <div class="empty-state">
                                        <i class="fas fa-box"></i>
                                        <p>Belum ada produk</p>
                                        <small>Klik "Tambah Produk" untuk menambahkan produk pertama</small>
                                    </div>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </div>
            </div>

            <!-- Analytics Content -->
            <div id="analyticsContent" class="page-content">
                <div class="analytics-filters">
                    <div class="filter-group">
                        <label for="analyticsPeriod">Periode Waktu</label>
                        <select id="analyticsPeriod" class="form-control">
                            <option value="7days">7 Hari Terakhir</option>
                            <option value="30days">30 Hari Terakhir</option>
                            <option value="90days">90 Hari Terakhir</option>
                            <option value="1year">1 Tahun Terakhir</option>
                        </select>
                    </div>
                    <div class="filter-group">
                        <label for="analyticsType">Jenis Analitik</label>
                        <select id="analyticsType" class="form-control">
                            <option value="users">Data Pengguna</option>
                            <option value="products">Data Produk</option>
                            <option value="revenue">Data Pendapatan</option>
                        </select>
                    </div>
                    <div class="filter-group">
                        <label for="chartType">Jenis Grafik</label>
                        <select id="chartType" class="form-control">
                            <option value="bar">Batang</option>
                            <option value="line">Garis</option>
                            <option value="pie">Pie</option>
                            <option value="doughnut">Donat</option>
                        </select>
                    </div>
                    <div class="filter-group">
                        <button class="btn btn-primary" id="applyAnalyticsFilters" style="margin-top: 22px;">
                            <i class="fas fa-filter"></i> Terapkan
                        </button>
                    </div>
                </div>

                <div class="content-grid">
                    <div class="grid-item">
                        <h3 data-lang="trafficAnalytics">Analitik Data</h3>
                        <div class="chart-container">
                            <canvas id="analyticsChart"></canvas>
                        </div>
                    </div>
                    <div class="grid-item">
                        <h3 data-lang="conversionRates">Statistik Detail</h3>
                        <div id="analyticsStats" style="margin-top: 20px;">
                            <div class="empty-state">
                                <i class="fas fa-chart-bar"></i>
                                <p>Belum ada data analitik</p>
                                <small>Data analitik akan muncul setelah ada data</small>
                            </div>
                        </div>
                    </div>
                </div>

                <div class="content-grid" style="margin-top: 20px;">
                    <div class="grid-item">
                        <h3>Distribusi Pengguna Berdasarkan Peran</h3>
                        <div class="chart-container">
                            <canvas id="userRoleChart"></canvas>
                        </div>
                    </div>
                    <div class="grid-item">
                        <h3>Distribusi Produk Berdasarkan Kategori</h3>
                        <div class="chart-container">
                            <canvas id="productCategoryChart"></canvas>
                        </div>
                    </div>
                </div>
            </div>

            <!-- Settings Content -->
            <div id="settingsContent" class="page-content">
                <div class="content-grid">
                    <div class="grid-item">
                        <h3 data-lang="appSettings">Pengaturan Aplikasi</h3>
                        <div class="form-group">
                            <label data-lang="theme">Tema</label>
                            <select class="form-control" id="themeSelect">
                                <option value="light" data-lang="lightTheme">Tema Terang</option>
                                <option value="dark" data-lang="darkTheme">Tema Gelap</option>
                            </select>
                        </div>
                        <div class="form-group">
                            <label data-lang="language">Bahasa</label>
                            <select class="form-control" id="languageSelect">
                                <option value="id">Bahasa Indonesia</option>
                                <option value="en">English</option>
                            </select>
                        </div>
                        <div class="form-group">
                            <label data-lang="notifications">Notifikasi</label>
                            <div>
                                <input type="checkbox" id="emailNotifications" checked>
                                <label for="emailNotifications" data-lang="emailNotifications">Notifikasi Email</label>
                            </div>
                            <div>
                                <input type="checkbox" id="pushNotifications" checked>
                                <label for="pushNotifications" data-lang="pushNotifications">Notifikasi Push</label>
                            </div>
                        </div>
                        <div class="form-actions">
                            <button class="btn btn-primary" id="saveSettings" data-lang="saveSettings">Simpan Pengaturan</button>
                        </div>
                    </div>
                    <div class="grid-item">
                        <h3 data-lang="systemInfo">Informasi Sistem</h3>
                        <div class="system-info">
                            <p><strong data-lang="version">Versi:</strong> 2.1.0</p>
                            <p><strong data-lang="lastUpdate">Pembaruan Terakhir:</strong> <span id="lastUpdateDate"></span></p>
                            <p><strong data-lang="serverStatus">Status Server:</strong> <span class="status active" data-lang="online">Online</span></p>
                            <p><strong data-lang="storage">Penyimpanan:</strong> <span id="storageUsage">0 GB / 5 GB</span></p>
                        </div>
                    </div>
                </div>
            </div>
        </main>
    </div>

    <!-- Add User Modal -->
    <div class="modal" id="addUserModal">
        <div class="modal-content">
            <div class="modal-header">
                <h3 data-lang="addUser">Tambah Pengguna</h3>
                <button class="close-modal">&times;</button>
            </div>
            <div class="modal-body">
                <form id="userForm">
                    <div class="form-group">
                        <label for="userName" data-lang="name">Nama</label>
                        <input type="text" id="userName" class="form-control" required>
                    </div>
                    <div class="form-group">
                        <label for="userEmail" data-lang="email">Email</label>
                        <input type="email" id="userEmail" class="form-control" required>
                    </div>
                    <div class="form-group">
                        <label for="userRole" data-lang="role">Peran</label>
                        <select id="userRole" class="form-control" required>
                            <option value="admin" data-lang="admin">Admin</option>
                            <option value="user" data-lang="user">Pengguna</option>
                            <option value="editor" data-lang="editor">Editor</option>
                        </select>
                    </div>
                    <div class="form-group">
                        <label for="userStatus" data-lang="status">Status</label>
                        <select id="userStatus" class="form-control" required>
                            <option value="active" data-lang="active">Aktif</option>
                            <option value="inactive" data-lang="inactive">Tidak Aktif</option>
                        </select>
                    </div>
                    <div class="form-actions">
                        <button type="button" class="btn btn-secondary close-modal" data-lang="cancel">Batal</button>
                        <button type="submit" class="btn btn-primary" data-lang="save">Simpan</button>
                    </div>
                </form>
            </div>
        </div>
    </div>

    <!-- Add Product Modal -->
    <div class="modal" id="addProductModal">
        <div class="modal-content">
            <div class="modal-header">
                <h3 data-lang="addProduct">Tambah Produk</h3>
                <button class="close-modal">&times;</button>
            </div>
            <div class="modal-body">
                <form id="productForm">
                    <div class="form-group">
                        <label for="productName" data-lang="productName">Nama Produk</label>
                        <input type="text" id="productName" class="form-control" required>
                    </div>
                    <div class="form-group">
                        <label for="productCategory" data-lang="category">Kategori</label>
                        <select id="productCategory" class="form-control" required>
                            <option value="electronics" data-lang="electronics">Elektronik</option>
                            <option value="clothing" data-lang="clothing">Pakaian</option>
                            <option value="food" data-lang="food">Makanan</option>
                            <option value="books" data-lang="books">Buku</option>
                        </select>
                    </div>
                    <div class="form-group">
                        <label for="productPrice" data-lang="price">Harga</label>
                        <input type="number" id="productPrice" class="form-control" required>
                    </div>
                    <div class="form-group">
                        <label for="productStock" data-lang="stock">Stok</label>
                        <input type="number" id="productStock" class="form-control" required>
                    </div>
                    <div class="form-group">
                        <label for="productStatus" data-lang="status">Status</label>
                        <select id="productStatus" class="form-control" required>
                            <option value="active" data-lang="active">Aktif</option>
                            <option value="inactive" data-lang="inactive">Tidak Aktif</option>
                        </select>
                    </div>
                    <div class="form-actions">
                        <button type="button" class="btn btn-secondary close-modal" data-lang="cancel">Batal</button>
                        <button type="submit" class="btn btn-primary" data-lang="save">Simpan</button>
                    </div>
                </form>
            </div>
        </div>
    </div>

    <!-- Notification Modal -->
    <div class="modal" id="notificationModal">
        <div class="modal-content">
            <div class="modal-header">
                <h3 data-lang="sendNotification">Kirim Notifikasi</h3>
                <button class="close-modal">&times;</button>
            </div>
            <div class="modal-body">
                <form id="notificationForm">
                    <div class="form-group">
                        <label for="notificationTitle" data-lang="title">Judul</label>
                        <input type="text" id="notificationTitle" class="form-control" required>
                    </div>
                    <div class="form-group">
                        <label for="notificationMessage" data-lang="message">Pesan</label>
                        <textarea id="notificationMessage" class="form-control" rows="4" required></textarea>
                    </div>
                    <div class="form-group">
                        <label for="notificationType" data-lang="type">Tipe</label>
                        <select id="notificationType" class="form-control" required>
                            <option value="info" data-lang="info">Info</option>
                            <option value="warning" data-lang="warning">Peringatan</option>
                            <option value="success" data-lang="success">Sukses</option>
                            <option value="error" data-lang="error">Error</option>
                        </select>
                    </div>
                    <div class="form-actions">
                        <button type="button" class="btn btn-secondary close-modal" data-lang="cancel">Batal</button>
                        <button type="submit" class="btn btn-primary" data-lang="send">Kirim</button>
                    </div>
                </form>
            </div>
        </div>
    </div>

    <script>
        // Data untuk aplikasi - SEMUA DATA KOSONG
        const appData = {
            currentLanguage: 'id',
            currentTheme: 'light',
            editingUserId: null,
            editingProductId: null,
            users: [], // Data pengguna kosong
            products: [], // Data produk kosong
            activities: [], // Data aktivitas kosong
            salesData: generateEmptySalesData(), // Data penjualan kosong dengan tanggal real-time
            notifications: [],
            languages: {
                id: {
                    dashboard: 'Dashboard',
                    users: 'Pengguna',
                    products: 'Produk',
                    analytics: 'Analitik',
                    settings: 'Pengaturan',
                    logout: 'Keluar',
                    searchPlaceholder: 'Cari...',
                    addUser: 'Tambah Pengguna',
                    addProduct: 'Tambah Produk',
                    viewReports: 'Lihat Laporan',
                    sendNotification: 'Kirim Notifikasi',
                    totalUsers: 'Total Pengguna',
                    totalRevenue: 'Total Pendapatan',
                    orders: 'Pesanan',
                    growth: 'Pertumbuhan',
                    recentActivity: 'Aktivitas Terbaru',
                    recentUsers: 'Pengguna Terbaru',
                    name: 'Nama',
                    email: 'Email',
                    status: 'Status',
                    actions: 'Aksi',
                    userManagement: 'Manajemen Pengguna',
                    productManagement: 'Manajemen Produk',
                    role: 'Peran',
                    admin: 'Admin',
                    user: 'Pengguna',
                    editor: 'Editor',
                    active: 'Aktif',
                    inactive: 'Tidak Aktif',
                    trafficAnalytics: 'Analitik Lalu Lintas',
                    conversionRates: 'Tingkat Konversi',
                    chartPlaceholder: 'Grafik akan ditampilkan di sini',
                    appSettings: 'Pengaturan Aplikasi',
                    systemInfo: 'Informasi Sistem',
                    theme: 'Tema',
                    lightTheme: 'Tema Terang',
                    darkTheme: 'Tema Gelap',
                    language: 'Bahasa',
                    notifications: 'Notifikasi',
                    emailNotifications: 'Notifikasi Email',
                    pushNotifications: 'Notifikasi Push',
                    saveSettings: 'Simpan Pengaturan',
                    version: 'Versi',
                    lastUpdate: 'Pembaruan Terakhir',
                    serverStatus: 'Status Server',
                    storage: 'Penyimpanan',
                    online: 'Online',
                    cancel: 'Batal',
                    save: 'Simpan',
                    productName: 'Nama Produk',
                    category: 'Kategori',
                    price: 'Harga',
                    stock: 'Stok',
                    electronics: 'Elektronik',
                    clothing: 'Pakaian',
                    food: 'Makanan',
                    books: 'Buku',
                    title: 'Judul',
                    message: 'Pesan',
                    type: 'Tipe',
                    info: 'Info',
                    warning: 'Peringatan',
                    success: 'Sukses',
                    error: 'Error',
                    send: 'Kirim'
                },
                en: {
                    dashboard: 'Dashboard',
                    users: 'Users',
                    products: 'Products',
                    analytics: 'Analytics',
                    settings: 'Settings',
                    logout: 'Logout',
                    searchPlaceholder: 'Search...',
                    addUser: 'Add User',
                    addProduct: 'Add Product',
                    viewReports: 'View Reports',
                    sendNotification: 'Send Notification',
                    totalUsers: 'Total Users',
                    totalRevenue: 'Total Revenue',
                    orders: 'Orders',
                    growth: 'Growth',
                    recentActivity: 'Recent Activity',
                    recentUsers: 'Recent Users',
                    name: 'Name',
                    email: 'Email',
                    status: 'Status',
                    actions: 'Actions',
                    userManagement: 'User Management',
                    productManagement: 'Product Management',
                    role: 'Role',
                    admin: 'Admin',
                    user: 'User',
                    editor: 'Editor',
                    active: 'Active',
                    inactive: 'Inactive',
                    trafficAnalytics: 'Traffic Analytics',
                    conversionRates: 'Conversion Rates',
                    chartPlaceholder: 'Chart will be displayed here',
                    appSettings: 'App Settings',
                    systemInfo: 'System Info',
                    theme: 'Theme',
                    lightTheme: 'Light Theme',
                    darkTheme: 'Dark Theme',
                    language: 'Language',
                    notifications: 'Notifications',
                    emailNotifications: 'Email Notifications',
                    pushNotifications: 'Push Notifications',
                    saveSettings: 'Save Settings',
                    version: 'Version',
                    lastUpdate: 'Last Update',
                    serverStatus: 'Server Status',
                    storage: 'Storage',
                    online: 'Online',
                    cancel: 'Cancel',
                    save: 'Save',
                    productName: 'Product Name',
                    category: 'Category',
                    price: 'Price',
                    stock: 'Stock',
                    electronics: 'Electronics',
                    clothing: 'Clothing',
                    food: 'Food',
                    books: 'Books',
                    title: 'Title',
                    message: 'Message',
                    type: 'Type',
                    info: 'Info',
                    warning: 'Warning',
                    success: 'Success',
                    error: 'Error',
                    send: 'Send'
                }
            }
        };

        // Fungsi untuk menghasilkan data penjualan kosong dengan tanggal real-time
        function generateEmptySalesData() {
            const salesData = [];
            const today = new Date();
            
            // Generate data untuk 6 bulan terakhir
            for (let i = 5; i >= 0; i--) {
                const date = new Date(today);
                date.setMonth(today.getMonth() - i);
                
                salesData.push({
                    date: date.toISOString().split('T')[0],
                    revenue: 0,
                    orders: 0,
                    users: 0
                });
            }
            
            return salesData;
        }

        // Fungsi untuk mendapatkan tanggal Indonesia
        function getIndonesianDate() {
            const now = new Date();
            const options = { 
                year: 'numeric', 
                month: 'long', 
                day: 'numeric',
                timeZone: 'Asia/Jakarta'
            };
            return now.toLocaleDateString('id-ID', options);
        }

        // DOM Elements
        const menuToggle = document.getElementById('menuToggle');
        const sidebar = document.querySelector('.sidebar');
        const themeToggle = document.getElementById('themeToggle');
        const languageToggle = document.getElementById('languageToggle');
        const languageText = document.getElementById('languageText');
        const themeSelect = document.getElementById('themeSelect');
        const languageSelect = document.getElementById('languageSelect');
        const saveSettings = document.getElementById('saveSettings');
        const pageContents = document.querySelectorAll('.page-content');
        const sidebarItems = document.querySelectorAll('.sidebar-nav li');
        const modals = document.querySelectorAll('.modal');
        const closeModalButtons = document.querySelectorAll('.close-modal');
        const activityList = document.getElementById('activityList');
        const usersTableBody = document.getElementById('usersTableBody');
        const allUsersTableBody = document.getElementById('allUsersTableBody');
        const productsTableBody = document.getElementById('productsTableBody');
        const searchInput = document.getElementById('searchInput');
        const applyAnalyticsFilters = document.getElementById('applyAnalyticsFilters');
        const lastUpdateDate = document.getElementById('lastUpdateDate');

        // Chart instances
        let analyticsChart = null;
        let userRoleChart = null;
        let productCategoryChart = null;

        // Initialize App
        function initApp() {
            // Set tanggal real-time
            lastUpdateDate.textContent = getIndonesianDate();
            
            loadTheme();
            loadLanguage();
            setupEventListeners();
            loadDashboardData();
            loadUsersData();
            loadProductsData();
            initializeAnalytics();
        }

        // Setup Event Listeners
        function setupEventListeners() {
            // Menu toggle for mobile
            if (menuToggle) {
                menuToggle.addEventListener('click', toggleSidebar);
            }

            // Theme toggle
            if (themeToggle) {
                themeToggle.addEventListener('click', toggleTheme);
            }
            if (themeSelect) {
                themeSelect.addEventListener('change', changeTheme);
            }

            // Language toggle
            if (languageToggle) {
                languageToggle.addEventListener('click', toggleLanguage);
            }
            if (languageSelect) {
                languageSelect.addEventListener('change', changeLanguage);
            }

            // Save settings
            if (saveSettings) {
                saveSettings.addEventListener('click', saveAppSettings);
            }

            // Sidebar navigation
            sidebarItems.forEach(item => {
                if (item.dataset.page) {
                    item.addEventListener('click', () => switchPage(item.dataset.page));
                }
            });

            // Quick actions
            document.querySelectorAll('.action-btn').forEach(btn => {
                btn.addEventListener('click', () => {
                    const action = btn.dataset.action;
                    handleQuickAction(action);
                });
            });

            // Modal handling
            closeModalButtons.forEach(btn => {
                btn.addEventListener('click', closeModals);
            });

            modals.forEach(modal => {
                modal.addEventListener('click', (e) => {
                    if (e.target === modal) closeModals();
                });
            });

            // Form submissions
            const userForm = document.getElementById('userForm');
            if (userForm) {
                userForm.addEventListener('submit', handleUserForm);
            }

            const productForm = document.getElementById('productForm');
            if (productForm) {
                productForm.addEventListener('submit', handleProductForm);
            }

            const notificationForm = document.getElementById('notificationForm');
            if (notificationForm) {
                notificationForm.addEventListener('submit', handleNotificationForm);
            }

            // Search functionality
            if (searchInput) {
                searchInput.addEventListener('input', handleSearch);
            }

            // Analytics filters
            if (applyAnalyticsFilters) {
                applyAnalyticsFilters.addEventListener('click', updateAnalyticsCharts);
            }
        }

        // Toggle Sidebar for Mobile
        function toggleSidebar() {
            sidebar.classList.toggle('active');
        }

        // Theme Functions
        function loadTheme() {
            const savedTheme = localStorage.getItem('theme') || 'light';
            setTheme(savedTheme);
        }

        function toggleTheme() {
            const newTheme = document.body.classList.contains('dark-mode') ? 'light' : 'dark';
            setTheme(newTheme);
        }

        function changeTheme() {
            setTheme(themeSelect.value);
        }

        function setTheme(theme) {
            if (theme === 'dark') {
                document.body.classList.add('dark-mode');
                if (themeToggle) themeToggle.innerHTML = '<i class="fas fa-sun"></i>';
            } else {
                document.body.classList.remove('dark-mode');
                if (themeToggle) themeToggle.innerHTML = '<i class="fas fa-moon"></i>';
            }
            
            if (themeSelect) {
                themeSelect.value = theme;
            }
            
            localStorage.setItem('theme', theme);
            appData.currentTheme = theme;
        }

        // Language Functions
        function loadLanguage() {
            const savedLanguage = localStorage.getItem('language') || 'id';
            setLanguage(savedLanguage);
        }

        function toggleLanguage() {
            const newLanguage = appData.currentLanguage === 'id' ? 'en' : 'id';
            setLanguage(newLanguage);
        }

        function changeLanguage() {
            setLanguage(languageSelect.value);
        }

        function setLanguage(language) {
            appData.currentLanguage = language;
            localStorage.setItem('language', language);
            
            // Update all elements with data-lang attribute
            document.querySelectorAll('[data-lang]').forEach(element => {
                const key = element.getAttribute('data-lang');
                if (appData.languages[language] && appData.languages[language][key]) {
                    element.textContent = appData.languages[language][key];
                }
            });
            
            // Update language toggle button
            if (languageToggle && languageText) {
                languageText.textContent = language.toUpperCase();
            }
            
            if (languageSelect) {
                languageSelect.value = language;
            }
        }

        // Save Settings
        function saveAppSettings() {
            const theme = themeSelect.value;
            const language = languageSelect.value;
            const emailNotifications = document.getElementById('emailNotifications').checked;
            const pushNotifications = document.getElementById('pushNotifications').checked;
            
            setTheme(theme);
            setLanguage(language);
            
            // Save notification settings to localStorage
            localStorage.setItem('emailNotifications', emailNotifications);
            localStorage.setItem('pushNotifications', pushNotifications);
            
            alert('Pengaturan berhasil disimpan!');
        }

        // Page Navigation
        function switchPage(page) {
            // Update active sidebar item
            sidebarItems.forEach(item => {
                if (item.dataset.page === page) {
                    item.classList.add('active');
                } else {
                    item.classList.remove('active');
                }
            });
            
            // Show selected page content
            pageContents.forEach(content => {
                if (content.id === `${page}Content`) {
                    content.classList.add('active');
                } else {
                    content.classList.remove('active');
                }
            });
            
            // Update page title
            const pageTitle = document.querySelector('.header h1');
            if (pageTitle && appData.languages[appData.currentLanguage] && appData.languages[appData.currentLanguage][page]) {
                pageTitle.textContent = appData.languages[appData.currentLanguage][page];
            }
            
            // Initialize analytics when switching to analytics page
            if (page === 'analytics') {
                initializeAnalytics();
            }
        }

        // Quick Actions
        function handleQuickAction(action) {
            switch(action) {
                case 'addUser':
                    openAddUserModal();
                    break;
                case 'addProduct':
                    openAddProductModal();
                    break;
                case 'sendNotification':
                    openModal('notificationModal');
                    break;
                case 'viewReports':
                    switchPage('analytics');
                    break;
            }
        }

        // Modal Functions
        function openModal(modalId) {
            document.getElementById(modalId).classList.add('active');
        }

        function closeModals() {
            modals.forEach(modal => modal.classList.remove('active'));
            // Reset editing IDs
            appData.editingUserId = null;
            appData.editingProductId = null;
            // Reset form titles
            const userModalTitle = document.querySelector('#addUserModal h3');
            if (userModalTitle) userModalTitle.textContent = appData.languages[appData.currentLanguage].addUser;
            
            const productModalTitle = document.querySelector('#addProductModal h3');
            if (productModalTitle) productModalTitle.textContent = appData.languages[appData.currentLanguage].addProduct;
        }

        // Open Add User Modal
        function openAddUserModal() {
            appData.editingUserId = null;
            const form = document.getElementById('userForm');
            if (form) form.reset();
            openModal('addUserModal');
        }

        // Open Add Product Modal
        function openAddProductModal() {
            appData.editingProductId = null;
            const form = document.getElementById('productForm');
            if (form) form.reset();
            openModal('addProductModal');
        }

        // Open Edit User Modal
        function openEditUserModal(id) {
            const user = appData.users.find(u => u.id == id);
            if (user) {
                appData.editingUserId = id;
                document.getElementById('userName').value = user.name;
                document.getElementById('userEmail').value = user.email;
                document.getElementById('userRole').value = user.role;
                document.getElementById('userStatus').value = user.status;
                
                const modalTitle = document.querySelector('#addUserModal h3');
                if (modalTitle) modalTitle.textContent = 'Edit Pengguna';
                
                openModal('addUserModal');
            }
        }

        // Open Edit Product Modal
        function openEditProductModal(id) {
            const product = appData.products.find(p => p.id == id);
            if (product) {
                appData.editingProductId = id;
                document.getElementById('productName').value = product.name;
                document.getElementById('productCategory').value = product.category;
                document.getElementById('productPrice').value = product.price;
                document.getElementById('productStock').value = product.stock;
                document.getElementById('productStatus').value = product.status;
                
                const modalTitle = document.querySelector('#addProductModal h3');
                if (modalTitle) modalTitle.textContent = 'Edit Produk';
                
                openModal('addProductModal');
            }
        }

        // Data Loading Functions
        function loadDashboardData() {
            // Load activities
            if (activityList) {
                if (appData.activities.length === 0) {
                    // Show empty state
                    activityList.innerHTML = `
                        <div class="empty-state">
                            <i class="fas fa-inbox"></i>
                            <p>Tidak ada aktivitas terbaru</p>
                            <small>Data aktivitas akan muncul di sini</small>
                        </div>
                    `;
                } else {
                    activityList.innerHTML = '';
                    appData.activities.forEach(activity => {
                        const activityItem = document.createElement('div');
                        activityItem.className = 'activity-item';
                        activityItem.innerHTML = `
                            <div class="activity-icon">
                                <i class="fas fa-${activity.icon}"></i>
                            </div>
                            <div class="activity-content">
                                <p>${activity.content}</p>
                                <span>${activity.time}</span>
                            </div>
                        `;
                        activityList.appendChild(activityItem);
                    });
                }
            }
            
            // Load recent users
            if (usersTableBody) {
                if (appData.users.length === 0) {
                    // Show empty state
                    usersTableBody.innerHTML = `
                        <tr>
                            <td colspan="4" style="text-align: center; padding: 40px;">
                                <div class="empty-state">
                                    <i class="fas fa-users"></i>
                                    <p>Belum ada pengguna</p>
                                    <small>Pengguna yang ditambahkan akan muncul di sini</small>
                                </div>
                            </td>
                        </tr>
                    `;
                } else {
                    usersTableBody.innerHTML = '';
                    appData.users.slice(0, 3).forEach(user => {
                        const row = document.createElement('tr');
                        row.innerHTML = `
                            <td>
                                <div class="user-cell">
                                    <img src="${user.avatar}" alt="${user.name}">
                                    ${user.name}
                                </div>
                            </td>
                            <td>${user.email}</td>
                            <td><span class="status ${user.status}">${appData.languages[appData.currentLanguage][user.status]}</span></td>
                            <td>
                                <button class="btn-edit" data-id="${user.id}"><i class="fas fa-edit"></i></button>
                                <button class="btn-delete" data-id="${user.id}"><i class="fas fa-trash"></i></button>
                            </td>
                        `;
                        usersTableBody.appendChild(row);
                    });
                    
                    // Add event listeners to edit and delete buttons
                    document.querySelectorAll('.btn-edit').forEach(btn => {
                        btn.addEventListener('click', () => openEditUserModal(btn.dataset.id));
                    });
                    
                    document.querySelectorAll('.btn-delete').forEach(btn => {
                        btn.addEventListener('click', () => deleteUser(btn.dataset.id));
                    });
                }
            }
            
            // Update dashboard stats
            updateDashboardStats();
        }

        function loadUsersData() {
            if (allUsersTableBody) {
                if (appData.users.length === 0) {
                    // Show empty state
                    allUsersTableBody.innerHTML = `
                        <tr>
                            <td colspan="5" style="text-align: center; padding: 40px;">
                                <div class="empty-state">
                                    <i class="fas fa-users"></i>
                                    <p>Belum ada pengguna</p>
                                    <small>Klik "Tambah Pengguna" untuk menambahkan pengguna pertama</small>
                                </div>
                            </td>
                        </tr>
                    `;
                } else {
                    allUsersTableBody.innerHTML = '';
                    appData.users.forEach(user => {
                        const row = document.createElement('tr');
                        row.innerHTML = `
                            <td>
                                <div class="user-cell">
                                    <img src="${user.avatar}" alt="${user.name}">
                                    ${user.name}
                                </div>
                            </td>
                            <td>${user.email}</td>
                            <td>${appData.languages[appData.currentLanguage][user.role]}</td>
                            <td><span class="status ${user.status}">${appData.languages[appData.currentLanguage][user.status]}</span></td>
                            <td>
                                <button class="btn-edit" data-id="${user.id}"><i class="fas fa-edit"></i></button>
                                <button class="btn-delete" data-id="${user.id}"><i class="fas fa-trash"></i></button>
                            </td>
                        `;
                        allUsersTableBody.appendChild(row);
                    });
                    
                    // Add event listeners to edit and delete buttons
                    document.querySelectorAll('.btn-edit').forEach(btn => {
                        btn.addEventListener('click', () => openEditUserModal(btn.dataset.id));
                    });
                    
                    document.querySelectorAll('.btn-delete').forEach(btn => {
                        btn.addEventListener('click', () => deleteUser(btn.dataset.id));
                    });
                }
            }
        }

        function loadProductsData() {
            if (productsTableBody) {
                if (appData.products.length === 0) {
                    // Show empty state
                    productsTableBody.innerHTML = `
                        <tr>
                            <td colspan="6" style="text-align: center; padding: 40px;">
                                <div class="empty-state">
                                    <i class="fas fa-box"></i>
                                    <p>Belum ada produk</p>
                                    <small>Klik "Tambah Produk" untuk menambahkan produk pertama</small>
                                </div>
                            </td>
                        </tr>
                    `;
                } else {
                    productsTableBody.innerHTML = '';
                    appData.products.forEach(product => {
                        const row = document.createElement('tr');
                        row.innerHTML = `
                            <td>${product.name}</td>
                            <td>${appData.languages[appData.currentLanguage][product.category]}</td>
                            <td>$${product.price}</td>
                            <td>${product.stock}</td>
                            <td><span class="status ${product.status}">${appData.languages[appData.currentLanguage][product.status]}</span></td>
                            <td>
                                <button class="btn-edit" data-id="${product.id}"><i class="fas fa-edit"></i></button>
                                <button class="btn-delete" data-id="${product.id}"><i class="fas fa-trash"></i></button>
                            </td>
                        `;
                        productsTableBody.appendChild(row);
                    });
                    
                    // Add event listeners to edit and delete buttons
                    document.querySelectorAll('.btn-edit').forEach(btn => {
                        btn.addEventListener('click', () => openEditProductModal(btn.dataset.id));
                    });
                    
                    document.querySelectorAll('.btn-delete').forEach(btn => {
                        btn.addEventListener('click', () => deleteProduct(btn.dataset.id));
                    });
                }
            }
        }

        // Update Dashboard Stats
        function updateDashboardStats() {
            // Update user count
            document.getElementById('users-count').textContent = appData.users.length;
            
            // Calculate total revenue
            const totalRevenue = appData.products.reduce((sum, product) => sum + (product.price * product.stock), 0);
            document.getElementById('revenue-count').textContent = `$${totalRevenue.toLocaleString()}`;
            
            // Calculate total orders (using stock as proxy for orders)
            const totalOrders = appData.products.reduce((sum, product) => sum + product.stock, 0);
            document.getElementById('orders-count').textContent = totalOrders.toLocaleString();
            
            // Calculate growth (placeholder calculation)
            const growth = 0;
            document.getElementById('growth-count').textContent = `${growth}%`;
        }

        // Form Handlers
        function handleUserForm(e) {
            e.preventDefault();
            const formData = {
                name: document.getElementById('userName').value,
                email: document.getElementById('userEmail').value,
                role: document.getElementById('userRole').value,
                status: document.getElementById('userStatus').value
            };
            
            if (appData.editingUserId) {
                // Update existing user
                const userIndex = appData.users.findIndex(u => u.id == appData.editingUserId);
                if (userIndex !== -1) {
                    appData.users[userIndex] = { 
                        ...appData.users[userIndex], 
                        ...formData 
                    };
                }
            } else {
                // Add new user
                const newId = appData.users.length > 0 ? Math.max(...appData.users.map(u => u.id)) + 1 : 1;
                const newUser = {
                    id: newId,
                    ...formData,
                    avatar: `https://ui-avatars.com/api/?name=${encodeURIComponent(formData.name)}&background=667eea&color=fff`,
                    joinDate: new Date().toISOString().split('T')[0]
                };
                appData.users.push(newUser);
                
                // Add activity
                const newActivity = {
                    id: appData.activities.length + 1,
                    icon: 'user-plus',
                    content: `<strong>${formData.name}</strong> ditambahkan sebagai pengguna baru`,
                    time: 'Baru saja'
                };
                appData.activities.unshift(newActivity);
            }
            
            closeModals();
            loadUsersData();
            loadDashboardData();
            updateAnalyticsCharts();
            
            alert(appData.editingUserId ? 'Pengguna berhasil diperbarui!' : 'Pengguna berhasil ditambahkan!');
        }

        function handleProductForm(e) {
            e.preventDefault();
            const formData = {
                name: document.getElementById('productName').value,
                category: document.getElementById('productCategory').value,
                price: parseFloat(document.getElementById('productPrice').value),
                stock: parseInt(document.getElementById('productStock').value),
                status: document.getElementById('productStatus').value
            };
            
            if (appData.editingProductId) {
                // Update existing product
                const productIndex = appData.products.findIndex(p => p.id == appData.editingProductId);
                if (productIndex !== -1) {
                    appData.products[productIndex] = { 
                        ...appData.products[productIndex], 
                        ...formData 
                    };
                }
            } else {
                // Add new product
                const newId = appData.products.length > 0 ? Math.max(...appData.products.map(p => p.id)) + 1 : 1;
                const newProduct = {
                    id: newId,
                    ...formData,
                    createdDate: new Date().toISOString().split('T')[0]
                };
                appData.products.push(newProduct);
                
                // Add activity
                const newActivity = {
                    id: appData.activities.length + 1,
                    icon: 'box',
                    content: `<strong>${formData.name}</strong> ditambahkan sebagai produk baru`,
                    time: 'Baru saja'
                };
                appData.activities.unshift(newActivity);
            }
            
            closeModals();
            loadProductsData();
            updateDashboardStats();
            updateAnalyticsCharts();
            
            alert(appData.editingProductId ? 'Produk berhasil diperbarui!' : 'Produk berhasil ditambahkan!');
        }

        function handleNotificationForm(e) {
            e.preventDefault();
            const formData = {
                title: document.getElementById('notificationTitle').value,
                message: document.getElementById('notificationMessage').value,
                type: document.getElementById('notificationType').value
            };
            
            // Add to activities
            const newActivity = {
                id: appData.activities.length + 1,
                icon: 'bell',
                content: `<strong>${formData.title}</strong> - ${formData.message}`,
                time: 'Baru saja'
            };
            appData.activities.unshift(newActivity);
            
            closeModals();
            loadDashboardData();
            
            alert('Notifikasi berhasil dikirim!');
        }

        // Delete Functions
        function deleteUser(id) {
            const user = appData.users.find(u => u.id == id);
            if (user && confirm(`Hapus pengguna ${user.name}?`)) {
                appData.users = appData.users.filter(u => u.id != id);
                
                // Add activity
                const newActivity = {
                    id: appData.activities.length + 1,
                    icon: 'user-minus',
                    content: `<strong>${user.name}</strong> dihapus dari sistem`,
                    time: 'Baru saja'
                };
                appData.activities.unshift(newActivity);
                
                loadUsersData();
                loadDashboardData();
                updateAnalyticsCharts();
                
                alert('Pengguna berhasil dihapus!');
            }
        }

        function deleteProduct(id) {
            const product = appData.products.find(p => p.id == id);
            if (product && confirm(`Hapus produk ${product.name}?`)) {
                appData.products = appData.products.filter(p => p.id != id);
                
                // Add activity
                const newActivity = {
                    id: appData.activities.length + 1,
                    icon: 'trash',
                    content: `<strong>${product.name}</strong> dihapus dari katalog`,
                    time: 'Baru saja'
                };
                appData.activities.unshift(newActivity);
                
                loadProductsData();
                updateDashboardStats();
                updateAnalyticsCharts();
                
                alert('Produk berhasil dihapus!');
            }
        }

        // Search Functionality
        function handleSearch(e) {
            const searchTerm = e.target.value.toLowerCase();
            
            // Search in users table
            if (allUsersTableBody && appData.users.length > 0) {
                const rows = allUsersTableBody.querySelectorAll('tr');
                rows.forEach(row => {
                    const text = row.textContent.toLowerCase();
                    row.style.display = text.includes(searchTerm) ? '' : 'none';
                });
            }
            
            // Search in products table
            if (productsTableBody && appData.products.length > 0) {
                const rows = productsTableBody.querySelectorAll('tr');
                rows.forEach(row => {
                    const text = row.textContent.toLowerCase();
                    row.style.display = text.includes(searchTerm) ? '' : 'none';
                });
            }
        }

        // Analytics Functions
        function initializeAnalytics() {
            updateAnalyticsCharts();
        }

        function updateAnalyticsCharts() {
            const period = document.getElementById('analyticsPeriod').value;
            const type = document.getElementById('analyticsType').value;
            const chartType = document.getElementById('chartType').value;
            
            // Update main analytics chart
            updateMainAnalyticsChart(period, type, chartType);
            
            // Update user role distribution chart
            updateUserRoleChart();
            
            // Update product category distribution chart
            updateProductCategoryChart();
            
            // Update analytics stats
            updateAnalyticsStats();
        }

        function updateMainAnalyticsChart(period, type, chartType) {
            const ctx = document.getElementById('analyticsChart').getContext('2d');
            
            // Destroy existing chart if it exists
            if (analyticsChart) {
                analyticsChart.destroy();
            }
            
            // Prepare data based on selected type and period
            let labels = [];
            let data = [];
            let label = '';
            let backgroundColor = '';
            
            switch(type) {
                case 'users':
                    labels = appData.salesData.map(item => {
                        const date = new Date(item.date);
                        return date.toLocaleDateString('id-ID', { month: 'short', year: 'numeric' });
                    });
                    data = appData.salesData.map(item => item.users);
                    label = 'Pengguna';
                    backgroundColor = 'rgba(54, 162, 235, 0.5)';
                    break;
                case 'products':
                    // Group products by category for chart
                    const categories = {};
                    appData.products.forEach(product => {
                        if (!categories[product.category]) {
                            categories[product.category] = 0;
                        }
                        categories[product.category] += product.stock;
                    });
                    
                    labels = Object.keys(categories).map(cat => appData.languages[appData.currentLanguage][cat]);
                    data = Object.values(categories);
                    label = 'Stok Produk';
                    backgroundColor = 'rgba(255, 99, 132, 0.5)';
                    break;
                case 'revenue':
                    labels = appData.salesData.map(item => {
                        const date = new Date(item.date);
                        return date.toLocaleDateString('id-ID', { month: 'short', year: 'numeric' });
                    });
                    data = appData.salesData.map(item => item.revenue);
                    label = 'Pendapatan ($)';
                    backgroundColor = 'rgba(75, 192, 192, 0.5)';
                    break;
            }
            
            // Create new chart
            analyticsChart = new Chart(ctx, {
                type: chartType,
                data: {
                    labels: labels,
                    datasets: [{
                        label: label,
                        data: data,
                        backgroundColor: backgroundColor,
                        borderColor: backgroundColor.replace('0.5', '1'),
                        borderWidth: 1
                    }]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    plugins: {
                        legend: {
                            position: 'top',
                        },
                        title: {
                            display: true,
                            text: `Analitik ${label}`
                        }
                    }
                }
            });
        }

        function updateUserRoleChart() {
            const ctx = document.getElementById('userRoleChart').getContext('2d');
            
            // Destroy existing chart if it exists
            if (userRoleChart) {
                userRoleChart.destroy();
            }
            
            // Count users by role
            const roleCounts = {};
            appData.users.forEach(user => {
                if (!roleCounts[user.role]) {
                    roleCounts[user.role] = 0;
                }
                roleCounts[user.role]++;
            });
            
            const labels = Object.keys(roleCounts).map(role => appData.languages[appData.currentLanguage][role]);
            const data = Object.values(roleCounts);
            
            // If no users, show empty chart
            if (appData.users.length === 0) {
                labels.push('Tidak ada data');
                data.push(1);
            }
            
            // Create new chart
            userRoleChart = new Chart(ctx, {
                type: 'doughnut',
                data: {
                    labels: labels,
                    datasets: [{
                        data: data,
                        backgroundColor: [
                            'rgba(255, 99, 132, 0.7)',
                            'rgba(54, 162, 235, 0.7)',
                            'rgba(255, 206, 86, 0.7)',
                            'rgba(75, 192, 192, 0.7)',
                            'rgba(153, 102, 255, 0.7)'
                        ],
                        borderColor: [
                            'rgba(255, 99, 132, 1)',
                            'rgba(54, 162, 235, 1)',
                            'rgba(255, 206, 86, 1)',
                            'rgba(75, 192, 192, 1)',
                            'rgba(153, 102, 255, 1)'
                        ],
                        borderWidth: 1
                    }]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    plugins: {
                        legend: {
                            position: 'bottom',
                        },
                        title: {
                            display: true,
                            text: 'Distribusi Peran Pengguna'
                        }
                    }
                }
            });
        }

        function updateProductCategoryChart() {
            const ctx = document.getElementById('productCategoryChart').getContext('2d');
            
            // Destroy existing chart if it exists
            if (productCategoryChart) {
                productCategoryChart.destroy();
            }
            
            // Count products by category
            const categoryCounts = {};
            appData.products.forEach(product => {
                if (!categoryCounts[product.category]) {
                    categoryCounts[product.category] = 0;
                }
                categoryCounts[product.category]++;
            });
            
            const labels = Object.keys(categoryCounts).map(category => appData.languages[appData.currentLanguage][category]);
            const data = Object.values(categoryCounts);
            
            // If no products, show empty chart
            if (appData.products.length === 0) {
                labels.push('Tidak ada data');
                data.push(1);
            }
            
            // Create new chart
            productCategoryChart = new Chart(ctx, {
                type: 'pie',
                data: {
                    labels: labels,
                    datasets: [{
                        data: data,
                        backgroundColor: [
                            'rgba(255, 99, 132, 0.7)',
                            'rgba(54, 162, 235, 0.7)',
                            'rgba(255, 206, 86, 0.7)',
                            'rgba(75, 192, 192, 0.7)',
                            'rgba(153, 102, 255, 0.7)'
                        ],
                        borderColor: [
                            'rgba(255, 99, 132, 1)',
                            'rgba(54, 162, 235, 1)',
                            'rgba(255, 206, 86, 1)',
                            'rgba(75, 192, 192, 1)',
                            'rgba(153, 102, 255, 1)'
                        ],
                        borderWidth: 1
                    }]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    plugins: {
                        legend: {
                            position: 'bottom',
                        },
                        title: {
                            display: true,
                            text: 'Distribusi Kategori Produk'
                        }
                    }
                }
            });
        }

        function updateAnalyticsStats() {
            const statsContainer = document.getElementById('analyticsStats');
            
            // Calculate various statistics
            const totalUsers = appData.users.length;
            const activeUsers = appData.users.filter(user => user.status === 'active').length;
            const totalProducts = appData.products.length;
            const activeProducts = appData.products.filter(product => product.status === 'active').length;
            const totalRevenue = appData.products.reduce((sum, product) => sum + (product.price * product.stock), 0);
            const totalStock = appData.products.reduce((sum, product) => sum + product.stock, 0);
            
            // User role distribution
            const roleDistribution = {};
            appData.users.forEach(user => {
                if (!roleDistribution[user.role]) {
                    roleDistribution[user.role] = 0;
                }
                roleDistribution[user.role]++;
            });
            
            // Product category distribution
            const categoryDistribution = {};
            appData.products.forEach(product => {
                if (!categoryDistribution[product.category]) {
                    categoryDistribution[product.category] = 0;
                }
                categoryDistribution[product.category]++;
            });
            
            // Create stats HTML
            if (totalUsers === 0 && totalProducts === 0) {
                statsContainer.innerHTML = `
                    <div class="empty-state">
                        <i class="fas fa-chart-bar"></i>
                        <p>Belum ada data analitik</p>
                        <small>Data analitik akan muncul setelah ada data</small>
                    </div>
                `;
            } else {
                statsContainer.innerHTML = `
                    <div style="display: grid; grid-template-columns: 1fr 1fr; gap: 15px;">
                        <div style="background: var(--hover-color); padding: 15px; border-radius: 8px;">
                            <h4 style="margin-bottom: 10px;">Statistik Pengguna</h4>
                            <p><strong>Total Pengguna:</strong> ${totalUsers}</p>
                            <p><strong>Pengguna Aktif:</strong> ${activeUsers}</p>
                            <p><strong>Pengguna Non-Aktif:</strong> ${totalUsers - activeUsers}</p>
                            ${Object.keys(roleDistribution).length > 0 ? `
                            <p><strong>Distribusi Peran:</strong></p>
                            <ul style="margin-left: 20px;">
                                ${Object.entries(roleDistribution).map(([role, count]) => 
                                    `<li>${appData.languages[appData.currentLanguage][role]}: ${count}</li>`
                                ).join('')}
                            </ul>
                            ` : ''}
                        </div>
                        <div style="background: var(--hover-color); padding: 15px; border-radius: 8px;">
                            <h4 style="margin-bottom: 10px;">Statistik Produk</h4>
                            <p><strong>Total Produk:</strong> ${totalProducts}</p>
                            <p><strong>Produk Aktif:</strong> ${activeProducts}</p>
                            <p><strong>Produk Non-Aktif:</strong> ${totalProducts - activeProducts}</p>
                            <p><strong>Total Stok:</strong> ${totalStock}</p>
                            <p><strong>Total Nilai Inventaris:</strong> $${totalRevenue.toLocaleString()}</p>
                            ${Object.keys(categoryDistribution).length > 0 ? `
                            <p><strong>Distribusi Kategori:</strong></p>
                            <ul style="margin-left: 20px;">
                                ${Object.entries(categoryDistribution).map(([category, count]) => 
                                    `<li>${appData.languages[appData.currentLanguage][category]}: ${count}</li>`
                                ).join('')}
                            </ul>
                            ` : ''}
                        </div>
                    </div>
                `;
            }
        }

        // Initialize the app when DOM is loaded
        document.addEventListener('DOMContentLoaded', initApp);
    </script>
</body>
</html>
