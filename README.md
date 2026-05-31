<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Kasech Butchers POS</title>
    <style>
        * { margin: 0; padding: 0; box-sizing: border-box; }
        body { font-family: 'Segoe UI', Tahoma, sans-serif; background: #1a1a2e; color: #fff; min-height: 100vh; }
        .hidden { display: none !important; }
        
        .login-screen { display: flex; flex-direction: column; align-items: center; justify-content: center; min-height: 100vh; padding: 20px; }
        .logo { font-size: 28px; font-weight: bold; color: #e94560; margin-bottom: 10px; text-align: center; }
        .tagline { color: #aaa; margin-bottom: 30px; font-size: 14px; }
        .login-box { background: #16213e; padding: 30px; border-radius: 15px; width: 100%; max-width: 400px; }
        .login-box h2 { text-align: center; margin-bottom: 20px; color: #e94560; }
        .input-group { margin-bottom: 15px; }
        .input-group label { display: block; margin-bottom: 5px; font-size: 14px; color: #ccc; }
        .input-group input { width: 100%; padding: 12px; border: none; border-radius: 8px; background: #0f3460; color: #fff; font-size: 16px; }
        .btn { width: 100%; padding: 14px; border: none; border-radius: 8px; font-size: 16px; font-weight: bold; cursor: pointer; transition: 0.3s; }
        .btn-primary { background: #e94560; color: #fff; }
        .btn-primary:active { background: #c13651; }
        .btn-success { background: #0f9b6e; color: #fff; }
        .btn-warning { background: #f39c12; color: #fff; }
        .btn-danger { background: #e74c3c; color: #fff; }
        .btn-secondary { background: #34495e; color: #fff; }
        .error-msg { color: #e74c3c; text-align: center; margin-top: 10px; font-size: 14px; }
        
        .app-container { display: none; min-height: 100vh; }
        .header { background: #16213e; padding: 15px; display: flex; justify-content: space-between; align-items: center; position: sticky; top: 0; z-index: 100; }
        .header h1 { font-size: 18px; color: #e94560; }
        .user-info { font-size: 12px; color: #aaa; }
        .logout-btn { padding: 8px 15px; font-size: 12px; }
        
        .nav-tabs { display: flex; background: #0f3460; overflow-x: auto; position: sticky; top: 60px; z-index: 99; }
        .nav-tab { flex: 1; padding: 15px 10px; text-align: center; font-size: 12px; cursor: pointer; border-bottom: 3px solid transparent; white-space: nowrap; min-width: 80px; }
        .nav-tab.active { border-bottom-color: #e94560; color: #e94560; font-weight: bold; }
        
        .tab-content { display: none; padding: 15px; padding-bottom: 80px; }
        .tab-content.active { display: block; }
        
        .category-filter { display: flex; gap: 10px; margin-bottom: 15px; overflow-x: auto; padding-bottom: 5px; }
        .cat-btn { padding: 8px 15px; background: #0f3460; border: none; border-radius: 20px; color: #fff; font-size: 13px; cursor: pointer; white-space: nowrap; }
        .cat-btn.active { background: #e94560; }
        .items-grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(140px, 1fr)); gap: 10px; }
        .item-card { background: #16213e; border-radius: 10px; padding: 15px; text-align: center; cursor: pointer; transition: 0.2s; position: relative; }
        .item-card:active { transform: scale(0.98); }
        .item-card.low-stock { border: 2px solid #e74c3c; }
        .item-card .item-name { font-size: 14px; margin-bottom: 5px; font-weight: bold; }
        .item-card .item-price { color: #e94560; font-size: 16px; font-weight: bold; }
        .item-card .item-stock { font-size: 11px; color: #aaa; margin-top: 5px; }
        .item-card img { width: 60px; height: 60px; object-fit: cover; border-radius: 8px; margin-bottom: 8px; }
        
        .cart-section { background: #16213e; border-radius: 10px; padding: 15px; margin-top: 15px; }
        .cart-header { display: flex; justify-content: space-between; align-items: center; margin-bottom: 15px; }
        .cart-items { max-height: 200px; overflow-y: auto; }
        .cart-item { display: flex; justify-content: space-between; align-items: center; padding: 10px 0; border-bottom: 1px solid #0f3460; }
        .cart-item-info { flex: 1; }
        .cart-item-name { font-size: 14px; }
        .cart-item-price { font-size: 12px; color: #aaa; }
        .qty-controls { display: flex; align-items: center; gap: 10px; }
        .qty-btn { width: 30px; height: 30px; border-radius: 50%; border: none; background: #0f3460; color: #fff; font-size: 16px; cursor: pointer; }
        .cart-total { display: flex; justify-content: space-between; padding: 15px 0; font-size: 18px; font-weight: bold; border-top: 2px solid #e94560; margin-top: 10px; }
        
        .modal-overlay { position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: rgba(0,0,0,0.8); z-index: 1000; display: flex; align-items: center; justify-content: center; padding: 20px; }
        .modal { background: #16213e; border-radius: 15px; width: 100%; max-width: 500px; max-height: 90vh; overflow-y: auto; padding: 25px; }
        .modal h2 { text-align: center; margin-bottom: 20px; color: #e94560; }
        .payment-options { display: grid; grid-template-columns: 1fr 1fr; gap: 15px; margin-bottom: 20px; }
        .payment-option { padding: 20px; background: #0f3460; border-radius: 10px; text-align: center; cursor: pointer; border: 2px solid transparent; }
        .payment-option.selected { border-color: #e94560; }
        .cash-section, .mobile-section { margin-top: 20px; }
        .amount-display { font-size: 24px; text-align: center; margin: 15px 0; color: #e94560; font-weight: bold; }
        .numpad { display: grid; grid-template-columns: repeat(3, 1fr); gap: 10px; margin-top: 15px; }
        .numpad-btn { padding: 20px; font-size: 20px; border: none; border-radius: 10px; background: #0f3460; color: #fff; cursor: pointer; }
        .numpad-btn:active { background: #e94560; }
        .change-display { text-align: center; font-size: 20px; margin: 15px 0; padding: 15px; background: #0f9b6e; border-radius: 10px; }
        .camera-section { text-align: center; margin-top: 20px; }
        .camera-preview { width: 100%; max-width: 300px; height: 200px; background: #0f3460; border-radius: 10px; margin: 10px auto; display: flex; align-items: center; justify-content: center; overflow: hidden; }
        .camera-preview img, .camera-preview video { width: 100%; height: 100%; object-fit: cover; }
        .camera-btn { margin-top: 10px; }
        
        .inventory-header { display: flex; justify-content: space-between; margin-bottom: 15px; }
        .search-box { width: 100%; padding: 12px; border-radius: 8px; border: none; background: #0f3460; color: #fff; margin-bottom: 15px; font-size: 16px; }
        .inventory-list { display: flex; flex-direction: column; gap: 10px; }
        .inv-item { background: #16213e; padding: 15px; border-radius: 10px; display: flex; justify-content: space-between; align-items: center; }
        .inv-item-info { flex: 1; }
        .inv-item-name { font-weight: bold; margin-bottom: 5px; }
        .inv-item-details { font-size: 12px; color: #aaa; }
        .inv-actions { display: flex; gap: 8px; }
        .icon-btn { width: 35px; height: 35px; border-radius: 50%; border: none; cursor: pointer; font-size: 16px; }
        
        .form-group { margin-bottom: 15px; }
        .form-group label { display: block; margin-bottom: 5px; font-size: 14px; }
        .form-group input, .form-group select { width: 100%; padding: 12px; border: none; border-radius: 8px; background: #0f3460; color: #fff; font-size: 16px; }
        .form-group input[type="file"] { padding: 8px; }
        
        .stats-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 15px; margin-bottom: 20px; }
        .stat-card { background: #16213e; padding: 20px; border-radius: 10px; text-align: center; }
        .stat-value { font-size: 28px; font-weight: bold; color: #e94560; }
        .stat-label { font-size: 12px; color: #aaa; margin-top: 5px; }
        .chart-container { background: #16213e; border-radius: 10px; padding: 20px; margin-bottom: 15px; }
        .chart-bar { display: flex; align-items: center; margin-bottom: 10px; }
        .chart-label { width: 80px; font-size: 12px; }
        .chart-bar-fill { flex: 1; height: 25px; background: #0f3460; border-radius: 5px; overflow: hidden; margin: 0 10px; }
        .chart-bar-inner { height: 100%; background: #e94560; border-radius: 5px; transition: width 0.5s; }
        .chart-value { width: 50px; text-align: right; font-size: 12px; }
        
        /* Performance Graph Styles */
        .performance-graph { background: #16213e; border-radius: 10px; padding: 20px; margin-bottom: 15px; }
        .graph-header { display: flex; justify-content: space-between; align-items: center; margin-bottom: 15px; }
        .graph-filters { display: flex; gap: 10px; }
        .graph-filter-btn { padding: 6px 12px; background: #0f3460; border: none; border-radius: 15px; color: #fff; font-size: 12px; cursor: pointer; }
        .graph-filter-btn.active { background: #e94560; }
        .graph-canvas-container { position: relative; height: 250px; width: 100%; }
        .graph-svg { width: 100%; height: 100%; }
        .graph-line { fill: none; stroke: #e94560; stroke-width: 3; stroke-linecap: round; stroke-linejoin: round; }
        .graph-area { fill: rgba(233, 69, 96, 0.2); stroke: none; }
        .graph-dot { fill: #e94560; stroke: #fff; stroke-width: 2; }
        .graph-grid-line { stroke: #0f3460; stroke-width: 1; }
        .graph-axis-text { fill: #aaa; font-size: 10px; }
        .graph-tooltip { position: absolute; background: #0f3460; padding: 8px 12px; border-radius: 8px; font-size: 12px; pointer-events: none; opacity: 0; transition: opacity 0.2s; z-index: 10; }
        .graph-tooltip.show { opacity: 1; }
        
        .trans-list { display: flex; flex-direction: column; gap: 10px; }
        .trans-item { background: #16213e; padding: 15px; border-radius: 10px; }
        .trans-header { display: flex; justify-content: space-between; margin-bottom: 10px; }
        .trans-id { font-size: 12px; color: #aaa; }
        .trans-amount { font-size: 18px; font-weight: bold; color: #0f9b6e; }
        .trans-details { font-size: 12px; color: #aaa; }
        .trans-items { margin-top: 10px; padding-top: 10px; border-top: 1px solid #0f3460; }
        .trans-item-row { display: flex; justify-content: space-between; font-size: 13px; padding: 3px 0; }
        
        .toast { position: fixed; bottom: 80px; left: 50%; transform: translateX(-50%) translateY(100px); background: #0f9b6e; color: #fff; padding: 12px 25px; border-radius: 25px; font-size: 14px; z-index: 2000; opacity: 0; transition: all 0.3s; }
        .toast.show { opacity: 1; transform: translateX(-50%) translateY(0); }
        .toast.error { background: #e74c3c; }
        
        .admin-badge { background: #e94560; color: #fff; padding: 2px 8px; border-radius: 10px; font-size: 10px; margin-left: 5px; }
        
        .empty-state { text-align: center; color: #aaa; padding: 60px 20px; }
        .empty-state-icon { font-size: 48px; margin-bottom: 15px; }
        
        .cancel-btn { background: #e74c3c; color: #fff; padding: 5px 10px; border: none; border-radius: 5px; font-size: 12px; cursor: pointer; margin-right: 5px; }
        .refund-btn { background: #f39c12; color: #fff; padding: 5px 10px; border: none; border-radius: 5px; font-size: 12px; cursor: pointer; }
        
        @media (min-width: 768px) {
            .items-grid { grid-template-columns: repeat(auto-fill, minmax(160px, 1fr)); }
            .nav-tab { font-size: 14px; padding: 15px 20px; }
            .graph-canvas-container { height: 300px; }
        }
    </style>
</head>
<body>

    <!-- LOGIN SCREEN -->
    <div id="loginScreen" class="login-screen">
        <div class="logo">🥩 KASECH BUTCHERS</div>
        <div class="tagline">Premium Meat & Beverages POS</div>
        <div class="login-box">
            <h2>Staff Login</h2>
            <div class="input-group">
                <label>Username</label>
                <input type="text" id="loginUsername" placeholder="Enter username" autocomplete="off">
            </div>
            <div class="input-group">
                <label>Password</label>
                <input type="password" id="loginPassword" placeholder="Enter password">
            </div>
            <button class="btn btn-primary" onclick="login()">Login</button>
            <div id="loginError" class="error-msg"></div>
        </div>
    </div>

    <!-- MAIN APP -->
    <div id="appContainer" class="app-container">
        <div class="header">
            <div>
                <h1>🥩 KASECH BUTCHERS</h1>
                <div class="user-info">
                    <span id="currentUserName"></span>
                    <span id="adminBadge" class="admin-badge hidden">ADMIN</span>
                </div>
            </div>
            <button class="btn btn-secondary logout-btn" onclick="logout()">Logout</button>
        </div>

        <div class="nav-tabs" id="navTabs">
            <div class="nav-tab active" onclick="switchTab('pos')">🛒 POS</div>
            <div class="nav-tab" onclick="switchTab('transactions')">📋 Transactions</div>
            <div class="nav-tab admin-only hidden" onclick="switchTab('inventory')">📦 Inventory</div>
            <div class="nav-tab admin-only hidden" onclick="switchTab('stats')">📊 Statistics</div>
        </div>

        <!-- POS TAB -->
        <div id="tab-pos" class="tab-content active">
            <div class="category-filter" id="categoryFilter">
                <button class="cat-btn active" onclick="filterCategory('all')">All</button>
                <button class="cat-btn" onclick="filterCategory('meat')">Meat</button>
                <button class="cat-btn" onclick="filterCategory('beverage')">መጠጥ</button>
            </div>
            <div class="items-grid" id="itemsGrid"></div>
            
            <div class="cart-section" id="cartSection" style="display:none;">
                <div class="cart-header">
                    <h3>🛒 Current Order</h3>
                    <button class="btn btn-danger" style="width:auto;padding:8px 15px;font-size:12px;" onclick="clearCart()">Clear</button>
                </div>
                <div class="cart-items" id="cartItems"></div>
                <div class="cart-total">
                    <span>Total:</span>
                    <span id="cartTotal">0.00 ETB</span>
                </div>
                <button class="btn btn-success" onclick="openCheckout()" style="margin-top:10px;">💰 Checkout</button>
            </div>
        </div>

        <!-- TRANSACTIONS TAB -->
        <div id="tab-transactions" class="tab-content">
            <h2 style="margin-bottom:15px;">Today's Transactions</h2>
            <div class="trans-list" id="transList"></div>
        </div>

        <!-- INVENTORY TAB (Admin Only) -->
        <div id="tab-inventory" class="tab-content">
            <div class="inventory-header">
                <h2>Stock Management</h2>
                <button class="btn btn-success" style="width:auto;padding:8px 15px;" onclick="showAddItem()">+ Add Item</button>
            </div>
            <input type="text" class="search-box" id="invSearch" placeholder="Search items..." oninput="renderInventory()">
            <div class="inventory-list" id="inventoryList"></div>
            
            <div id="itemModal" class="modal-overlay hidden">
                <div class="modal">
                    <h2 id="itemModalTitle">Add New Item</h2>
                    <div class="form-group">
                        <label>Item Name</label>
                        <input type="text" id="itemName" placeholder="e.g. Beef Fillet">
                    </div>
                    <div class="form-group">
                        <label>Category</label>
                        <select id="itemCategory">
                            <option value="meat">Meat</option>
                            <option value="beverage">መጠጥ (Beverage)</option>
                        </select>
                    </div>
                    <div class="form-group">
                        <label>Price (ETB)</label>
                        <input type="number" id="itemPrice" placeholder="0.00" step="0.01">
                    </div>
                    <div class="form-group">
                        <label>Stock Quantity</label>
                        <input type="number" id="itemStock" placeholder="0">
                    </div>
                    <div class="form-group">
                        <label>Item Image</label>
                        <input type="file" id="itemImage" accept="image/*" onchange="previewImage(this)">
                        <div id="imagePreview" style="margin-top:10px;max-width:150px;"></div>
                    </div>
                    <button class="btn btn-success" onclick="saveItem()">Save Item</button>
                    <button class="btn btn-secondary" onclick="closeItemModal()" style="margin-top:10px;">Cancel</button>
                </div>
            </div>
        </div>

        <!-- STATISTICS TAB (Admin Only) -->
        <div id="tab-stats" class="tab-content">
            <h2 style="margin-bottom:15px;">Sales Analytics</h2>
            <div class="stats-grid">
                <div class="stat-card">
                    <div class="stat-value" id="totalRevenue">0</div>
                    <div class="stat-label">Total Revenue (ETB)</div>
                </div>
                <div class="stat-card">
                    <div class="stat-value" id="totalTransactions">0</div>
                    <div class="stat-label">Transactions Today</div>
                </div>
                <div class="stat-card">
                    <div class="stat-value" id="itemsSold">0</div>
                    <div class="stat-label">Items Sold</div>
                </div>
                <div class="stat-card">
                    <div class="stat-value" id="avgOrder">0</div>
                    <div class="stat-label">Avg Order (ETB)</div>
                </div>
            </div>
            
            <!-- Store Performance Graph -->
            <div class="performance-graph">
                <div class="graph-header">
                    <h3>📈 Store Performance</h3>
                    <div class="graph-filters">
                        <button class="graph-filter-btn active" onclick="setGraphPeriod('week')">Week</button>
                        <button class="graph-filter-btn" onclick="setGraphPeriod('month')">Month</button>
                        <button class="graph-filter-btn" onclick="setGraphPeriod('year')">Year</button>
                    </div>
                </div>
                <div class="graph-canvas-container" id="graphContainer">
                    <svg class="graph-svg" id="performanceGraph"></svg>
                    <div class="graph-tooltip" id="graphTooltip"></div>
                </div>
            </div>
            
            <div class="chart-container">
                <h3 style="margin-bottom:15px;">Top Selling Items</h3>
                <div id="topItemsChart"></div>
            </div>
            
            <div class="chart-container">
                <h3 style="margin-bottom:15px;">Sales by Day of Week</h3>
                <div id="dayOfWeekChart"></div>
            </div>
        </div>
    </div>

    <!-- CHECKOUT MODAL -->
    <div id="checkoutModal" class="modal-overlay hidden">
        <div class="modal">
            <h2>Checkout</h2>
            <div class="amount-display">Total: <span id="checkoutTotal">0.00</span> ETB</div>
            
            <div class="payment-options">
                <div class="payment-option" id="payCash" onclick="selectPayment('cash')">
                    <span style="font-size:30px;">💵</span>
                    <div>Cash</div>
                </div>
                <div class="payment-option" id="payMobile" onclick="selectPayment('mobile')">
                    <span style="font-size:30px;">📱</span>
                    <div>Mobile Banking</div>
                </div>
            </div>
            
            <div id="cashSection" class="cash-section hidden">
                <div class="amount-display">Amount Due: <span id="cashAmount">0.00</span> ETB</div>
                <div style="text-align:center;margin-bottom:10px;">Customer Paid:</div>
                <div class="numpad" id="numpad">
                    <button class="numpad-btn" onclick="addNum('1')">1</button>
                    <button class="numpad-btn" onclick="addNum('2')">2</button>
                    <button class="numpad-btn" onclick="addNum('3')">3</button>
                    <button class="numpad-btn" onclick="addNum('4')">4</button>
                    <button class="numpad-btn" onclick="addNum('5')">5</button>
                    <button class="numpad-btn" onclick="addNum('6')">6</button>
                    <button class="numpad-btn" onclick="addNum('7')">7</button>
                    <button class="numpad-btn" onclick="addNum('8')">8</button>
                    <button class="numpad-btn" onclick="addNum('9')">9</button>
                    <button class="numpad-btn" onclick="addNum('0')">0</button>
                    <button class="numpad-btn" onclick="addNum('00')">00</button>
                    <button class="numpad-btn" onclick="clearNum()">C</button>
                </div>
                <div style="text-align:center;margin:10px 0;font-size:20px;">
                    Paid: <span id="paidAmount">0</span> ETB
                </div>
                <div class="change-display" id="changeDisplay" style="display:none;">
                    Change: <span id="changeAmount">0.00</span> ETB
                </div>
                <button class="btn btn-success" id="completeCashBtn" onclick="completeCashPayment()" disabled>Complete Payment</button>
            </div>
            
            <div id="mobileSection" class="mobile-section hidden">
                <div class="amount-display">Amount: <span id="mobileAmount">0.00</span> ETB</div>
                <div style="text-align:center;margin-bottom:10px;">Upload Payment Screenshot</div>
                <div class="camera-section">
                    <div class="camera-preview" id="cameraPreview">
                        <span style="color:#aaa;">No image captured</span>
                    </div>
                    <input type="file" id="mobileScreenshot" accept="image/*" capture="environment" style="display:none;" onchange="handleScreenshot(this)">
                    <button class="btn btn-primary camera-btn" onclick="document.getElementById('mobileScreenshot').click()">📷 Take Photo / Upload</button>
                    <button class="btn btn-success" id="completeMobileBtn" onclick="completeMobilePayment()" style="margin-top:10px;" disabled>Complete Payment</button>
                </div>
            </div>
            
            <button class="btn btn-secondary" onclick="closeCheckout()" style="margin-top:15px;">Cancel</button>
        </div>
    </div>

    <!-- ADMIN AUTH MODAL -->
    <div id="adminAuthModal" class="modal-overlay hidden">
        <div class="modal">
            <h2>Admin Authentication Required</h2>
            <p style="text-align:center;color:#aaa;margin-bottom:15px;">This action requires admin privileges</p>
            <div class="input-group">
                <label>Admin Password</label>
                <input type="password" id="adminAuthPassword" placeholder="Enter admin password">
            </div>
            <button class="btn btn-primary" onclick="verifyAdminAuth()">Verify</button>
            <button class="btn btn-secondary" onclick="closeAdminAuth()" style="margin-top:10px;">Cancel</button>
            <div id="adminAuthError" class="error-msg"></div>
        </div>
    </div>

    <div class="toast" id="toast"></div>

    <script>
        // ==================== DATA & STATE ====================
        const defaultUsers = [
            { username: 'admin', password: 'admin123', role: 'admin', name: 'Administrator' },
            { username: 'cashier1', password: 'cash123', role: 'cashier', name: 'Cashier One' },
            { username: 'cashier2', password: 'cash123', role: 'cashier', name: 'Cashier Two' }
        ];

        const defaultItems = [];

        let users = JSON.parse(localStorage.getItem('kb_users')) || [...defaultUsers];
        let items = JSON.parse(localStorage.getItem('kb_items')) || [...defaultItems];
        let transactions = JSON.parse(localStorage.getItem('kb_transactions')) || [];
        let currentUser = null;
        let cart = [];
        let currentCategory = 'all';
        let editingItemId = null;
        let pendingAdminAction = null;
        let paymentMethod = null;
        let cashPaid = '';
        let mobileScreenshotData = null;
        let graphPeriod = 'week';

        // ==================== AUTHENTICATION ====================
        function login() {
            const username = document.getElementById('loginUsername').value.trim();
            const password = document.getElementById('loginPassword').value;
            const errorDiv = document.getElementById('loginError');
            
            const user = users.find(u => u.username === username && u.password === password);
            
            if (!user) {
                errorDiv.textContent = 'Invalid username or password!';
                return;
            }
            
            currentUser = user;
            errorDiv.textContent = '';
            localStorage.setItem('kb_currentUser', JSON.stringify(user));
            
            document.getElementById('loginScreen').style.display = 'none';
            document.getElementById('appContainer').style.display = 'block';
            document.getElementById('currentUserName').textContent = user.name;
            
            if (user.role === 'admin') {
                document.getElementById('adminBadge').classList.remove('hidden');
                document.querySelectorAll('.admin-only').forEach(el => el.classList.remove('hidden'));
            }
            
            initApp();
        }

        function logout() {
            currentUser = null;
            cart = [];
            localStorage.removeItem('kb_currentUser');
            document.getElementById('loginScreen').style.display = 'flex';
            document.getElementById('appContainer').style.display = 'none';
            document.getElementById('loginUsername').value = '';
            document.getElementById('loginPassword').value = '';
            document.getElementById('adminBadge').classList.add('hidden');
            document.querySelectorAll('.admin-only').forEach(el => el.classList.add('hidden'));
        }

        window.onload = function() {
            const saved = localStorage.getItem('kb_currentUser');
            if (saved) {
                currentUser = JSON.parse(saved);
                document.getElementById('loginScreen').style.display = 'none';
                document.getElementById('appContainer').style.display = 'block';
                document.getElementById('currentUserName').textContent = currentUser.name;
                if (currentUser.role === 'admin') {
                    document.getElementById('adminBadge').classList.remove('hidden');
                    document.querySelectorAll('.admin-only').forEach(el => el.classList.remove('hidden'));
                }
                initApp();
            }
        };

        // ==================== APP INITIALIZATION ====================
        function initApp() {
            renderItems();
            renderTransactions();
            if (currentUser.role === 'admin') {
                renderInventory();
                renderStatistics();
            }
        }

        function switchTab(tabName) {
            document.querySelectorAll('.tab-content').forEach(t => t.classList.remove('active'));
            document.querySelectorAll('.nav-tab').forEach(t => t.classList.remove('active'));
            
            document.getElementById('tab-' + tabName).classList.add('active');
            event.target.classList.add('active');
            
            if (tabName === 'stats') renderStatistics();
            if (tabName === 'transactions') renderTransactions();
            if (tabName === 'inventory') renderInventory();
        }

        // ==================== POS FUNCTIONS ====================
        function filterCategory(cat) {
            currentCategory = cat;
            document.querySelectorAll('.cat-btn').forEach(btn => btn.classList.remove('active'));
            event.target.classList.add('active');
            renderItems();
        }

        function renderItems() {
            const grid = document.getElementById('itemsGrid');
            const filtered = currentCategory === 'all' ? items : items.filter(i => i.category === currentCategory);
            
            if (filtered.length === 0) {
                grid.innerHTML = `
                    <div class="empty-state" style="grid-column:1/-1;">
                        <div class="empty-state-icon">📦</div>
                        <div>No items in inventory</div>
                        <div style="font-size:12px;margin-top:10px;">${currentUser.role === 'admin' ? 'Go to Inventory tab to add items' : 'Ask admin to add items'}</div>
                    </div>
                `;
                return;
            }
            
            grid.innerHTML = filtered.map(item => `
                <div class="item-card ${item.stock <= 5 ? 'low-stock' : ''}" onclick="addToCart(${item.id})">
                    ${item.image ? `<img src="${item.image}" alt="${item.name}">` : `<div style="width:60px;height:60px;background:#0f3460;border-radius:8px;margin:0 auto 8px;display:flex;align-items:center;justify-content:center;font-size:24px;">${item.category === 'meat' ? '🥩' : '🥤'}</div>`}
                    <div class="item-name">${item.name}</div>
                    <div class="item-price">${item.price.toFixed(2)} ETB</div>
                    <div class="item-stock">Stock: ${item.stock}</div>
                </div>
            `).join('');
        }

        function addToCart(itemId) {
            const item = items.find(i => i.id === itemId);
            if (!item || item.stock <= 0) {
                showToast('Item out of stock!', true);
                return;
            }
            
            const existing = cart.find(c => c.id === itemId);
            if (existing) {
                if (existing.qty >= item.stock) {
                    showToast('Maximum stock reached!', true);
                    return;
                }
                existing.qty++;
            } else {
                cart.push({ id: item.id, name: item.name, price: item.price, qty: 1 });
            }
            
            renderCart();
            showToast(`${item.name} added to cart`);
        }

        function renderCart() {
            const section = document.getElementById('cartSection');
            const list = document.getElementById('cartItems');
            const totalEl = document.getElementById('cartTotal');
            
            if (cart.length === 0) {
                section.style.display = 'none';
                return;
            }
            
            section.style.display = 'block';
            list.innerHTML = cart.map(item => `
                <div class="cart-item">
                    <div class="cart-item-info">
                        <div class="cart-item-name">${item.name}</div>
                        <div class="cart-item-price">${item.price.toFixed(2)} ETB each</div>
                    </div>
                    <div class="qty-controls">
                        <button class="qty-btn" onclick="updateQty(${item.id}, -1)">-</button>
                        <span>${item.qty}</span>
                        <button class="qty-btn" onclick="updateQty(${item.id}, 1)">+</button>
                    </div>
                    <div style="min-width:80px;text-align:right;">
                        <div style="font-weight:bold;">${(item.price * item.qty).toFixed(2)} ETB</div>
                    </div>
                </div>
            `).join('');
            
            const total = cart.reduce((sum, item) => sum + (item.price * item.qty), 0);
            totalEl.textContent = total.toFixed(2) + ' ETB';
        }

        function updateQty(itemId, delta) {
            const item = items.find(i => i.id === itemId);
            const cartItem = cart.find(c => c.id === itemId);
            
            if (!cartItem) return;
            
            const newQty = cartItem.qty + delta;
            if (newQty <= 0) {
                cart = cart.filter(c => c.id !== itemId);
            } else if (newQty > item.stock) {
                showToast('Not enough stock!', true);
                return;
            } else {
                cartItem.qty = newQty;
            }
            
            renderCart();
        }

        function clearCart() {
            cart = [];
            renderCart();
        }

        // ==================== CHECKOUT ====================
        function openCheckout() {
            if (cart.length === 0) {
                showToast('Cart is empty!', true);
                return;
            }
            
            const total = cart.reduce((sum, item) => sum + (item.price * item.qty), 0);
            document.getElementById('checkoutTotal').textContent = total.toFixed(2);
            document.getElementById('cashAmount').textContent = total.toFixed(2);
            document.getElementById('mobileAmount').textContent = total.toFixed(2);
            
            paymentMethod = null;
            cashPaid = '';
            mobileScreenshotData = null;
            
            document.querySelectorAll('.payment-option').forEach(p => p.classList.remove('selected'));
            document.getElementById('cashSection').classList.add('hidden');
            document.getElementById('mobileSection').classList.add('hidden');
            document.getElementById('paidAmount').textContent = '0';
            document.getElementById('changeDisplay').style.display = 'none';
            document.getElementById('completeCashBtn').disabled = true;
            document.getElementById('completeMobileBtn').disabled = true;
            document.getElementById('cameraPreview').innerHTML = '<span style="color:#aaa;">No image captured</span>';
            
            document.getElementById('checkoutModal').classList.remove('hidden');
        }

        function selectPayment(method) {
            paymentMethod = method;
            document.querySelectorAll('.payment-option').forEach(p => p.classList.remove('selected'));
            document.getElementById('pay' + (method === 'cash' ? 'Cash' : 'Mobile')).classList.add('selected');
            
            document.getElementById('cashSection').classList.add('hidden');
            document.getElementById('mobileSection').classList.add('hidden');
            
            if (method === 'cash') {
                document.getElementById('cashSection').classList.remove('hidden');
                cashPaid = '';
                updateCashDisplay();
            } else {
                document.getElementById('mobileSection').classList.remove('hidden');
            }
        }

        function addNum(num) {
            cashPaid += num;
            updateCashDisplay();
        }

        function clearNum() {
            cashPaid = '';
            updateCashDisplay();
        }

        function updateCashDisplay() {
            const paid = cashPaid === '' ? 0 : parseInt(cashPaid);
            const total = parseFloat(document.getElementById('checkoutTotal').textContent);
            
            document.getElementById('paidAmount').textContent = paid;
            
            if (paid >= total) {
                const change = paid - total;
                document.getElementById('changeAmount').textContent = change.toFixed(2);
                document.getElementById('changeDisplay').style.display = 'block';
                document.getElementById('completeCashBtn').disabled = false;
            } else {
                document.getElementById('changeDisplay').style.display = 'none';
                document.getElementById('completeCashBtn').disabled = true;
            }
        }

        function handleScreenshot(input) {
            const file = input.files[0];
            if (!file) return;
            
            const reader = new FileReader();
            reader.onload = function(e) {
                mobileScreenshotData = e.target.result;
                document.getElementById('cameraPreview').innerHTML = `<img src="${e.target.result}" style="width:100%;height:100%;object-fit:cover;">`;
                document.getElementById('completeMobileBtn').disabled = false;
            };
            reader.readAsDataURL(file);
        }

        function completeCashPayment() {
            const total = parseFloat(document.getElementById('checkoutTotal').textContent);
            const paid = parseInt(cashPaid);
            const change = paid - total;
            
            completeTransaction('cash', total, paid, change);
        }

        function completeMobilePayment() {
            const total = parseFloat(document.getElementById('checkoutTotal').textContent);
            completeTransaction('mobile', total, 0, 0, mobileScreenshotData);
        }

        function completeTransaction(method, total, paid, change, screenshot = null) {
            // Deduct stock
            cart.forEach(cartItem => {
                const item = items.find(i => i.id === cartItem.id);
                if (item) {
                    item.stock -= cartItem.qty;
                }
            });
            
            const transaction = {
                id: 'TXN' + Date.now(),
                date: new Date().toISOString(),
                items: [...cart],
                total: total,
                paymentMethod: method,
                paid: paid,
                change: change,
                cashier: currentUser.name,
                screenshot: screenshot,
                status: 'completed'
            };
            
            transactions.push(transaction);
            
            localStorage.setItem('kb_items', JSON.stringify(items));
            localStorage.setItem('kb_transactions', JSON.stringify(transactions));
            
            cart = [];
            renderCart();
            renderItems();
            
            closeCheckout();
            showToast(`Payment complete! ${method === 'cash' ? 'Change: ' + change.toFixed(2) + ' ETB' : 'Mobile banking confirmed'}`);
            
            renderTransactions();
            if (currentUser.role === 'admin') renderStatistics();
        }

        function closeCheckout() {
            document.getElementById('checkoutModal').classList.add('hidden');
        }

        // ==================== TRANSACTIONS ====================
        function renderTransactions() {
            const list = document.getElementById('transList');
            const today = new Date().toDateString();
            const todayTrans = transactions.filter(t => new Date(t.date).toDateString() === today);
            
            if (todayTrans.length === 0) {
                list.innerHTML = `
                    <div class="empty-state">
                        <div class="empty-state-icon">📋</div>
                        <div>No transactions today</div>
                    </div>
                `;
                return;
            }
            
            list.innerHTML = todayTrans.reverse().map(t => `
                <div class="trans-item" style="${t.status === 'cancelled' ? 'opacity:0.6;border-left:3px solid #e74c3c;' : ''}">
                    <div class="trans-header">
                        <div>
                            <div class="trans-id">${t.id} ${t.status === 'cancelled' ? '<span style="color:#e74c3c;">[CANCELLED]</span>' : ''}</div>
                            <div class="trans-details">${new Date(t.date).toLocaleTimeString()} | ${t.cashier}</div>
                        </div>
                        <div class="trans-amount" style="${t.status === 'cancelled' ? 'text-decoration:line-through;color:#e74c3c;' : ''}">${t.total.toFixed(2)} ETB</div>
                    </div>
                    <div style="font-size:12px;color:#e94560;margin-bottom:5px;">
                        ${t.paymentMethod === 'cash' ? '💵 Cash' : '📱 Mobile Banking'} 
                        ${t.paymentMethod === 'cash' ? `(Paid: ${t.paid.toFixed(2)}, Change: ${t.change.toFixed(2)})` : ''}
                    </div>
                    <div class="trans-items">
                        ${t.items.map(i => `
                            <div class="trans-item-row">
                                <span>${i.name} x${i.qty}</span>
                                <span>${(i.price * i.qty).toFixed(2)} ETB</span>
                            </div>
                        `).join('')}
                    </div>
                    ${currentUser.role === 'admin' && t.status !== 'cancelled' ? `
                        <div style="margin-top:10px;text-align:right;">
                            <button class="cancel-btn" onclick="requestCancelTransaction('${t.id}')">Cancel</button>
                            <button class="refund-btn" onclick="requestRefund('${t.id}')">Refund</button>
                        </div>
                    ` : ''}
                </div>
            `).join('');
        }

        // ==================== CANCEL TRANSACTION (Admin Only) ====================
        function requestCancelTransaction(txnId) {
            if (currentUser.role !== 'admin') {
                showToast('Admin access required!', true);
                return;
            }
            
            pendingAdminAction = () => cancelTransaction(txnId);
            document.getElementById('adminAuthModal').classList.remove('hidden');
            document.getElementById('adminAuthPassword').value = '';
            document.getElementById('adminAuthError').textContent = '';
        }

        function cancelTransaction(txnId) {
            const txn = transactions.find(t => t.id === txnId);
            if (!txn || txn.status === 'cancelled') return;
            
            // RESTORE STOCK - This is the fix!
            txn.items.forEach(item => {
                const invItem = items.find(i => i.id === item.id);
                if (invItem) {
                    invItem.stock += item.qty;
                }
            });
            
            // Mark as cancelled
            txn.status = 'cancelled';
            txn.cancelledDate = new Date().toISOString();
            txn.cancelledBy = currentUser.name;
            
            localStorage.setItem('kb_items', JSON.stringify(items));
            localStorage.setItem('kb_transactions', JSON.stringify(transactions));
            
            renderTransactions();
            renderItems();
            renderInventory();
            renderStatistics();
            showToast('Transaction cancelled - stock restored');
        }

        // ==================== REFUND (Admin Only) ====================
        function requestRefund(txnId) {
            if (currentUser.role !== 'admin') {
                showToast('Admin access required!', true);
                return;
            }
            
            pendingAdminAction = () => processRefund(txnId);
            document.getElementById('adminAuthModal').classList.remove('hidden');
            document.getElementById('adminAuthPassword').value = '';
            document.getElementById('adminAuthError').textContent = '';
        }

        function processRefund(txnId) {
            const txn = transactions.find(t => t.id === txnId);
            if (!txn || txn.refunded) return;
            
            // Restore stock
            txn.items.forEach(item => {
                const invItem = items.find(i => i.id === item.id);
                if (invItem) {
                    invItem.stock += item.qty;
                }
            });
            
            txn.refunded = true;
            txn.refundDate = new Date().toISOString();
            txn.refundedBy = currentUser.name;
            
            localStorage.setItem('kb_items', JSON.stringify(items));
            localStorage.setItem('kb_transactions', JSON.stringify(transactions));
            
            renderTransactions();
            renderItems();
            renderInventory();
            renderStatistics();
            showToast('Transaction refunded - stock restored');
        }

        // ==================== INVENTORY MANAGEMENT ====================
        function renderInventory() {
            const list = document.getElementById('inventoryList');
            const search = document.getElementById('invSearch').value.toLowerCase();
            const filtered = items.filter(i => i.name.toLowerCase().includes(search));
            
            if (filtered.length === 0) {
                list.innerHTML = `
                    <div class="empty-state">
                        <div class="empty-state-icon">📦</div>
                        <div>No items in inventory</div>
                        <div style="font-size:12px;margin-top:10px;">Click "Add Item" to get started</div>
                    </div>
                `;
                return;
            }
            
            list.innerHTML = filtered.map(item => `
                <div class="inv-item">
                    <div class="inv-item-info">
                        <div class="inv-item-name">${item.name}</div>
                        <div class="inv-item-details">
                            ${item.category === 'meat' ? '🥩' : '🥤'} ${item.category === 'beverage' ? 'መጠጥ' : 'Meat'} | 
                            ${item.price.toFixed(2)} ETB | Stock: ${item.stock}
                        </div>
                    </div>
                    <div class="inv-actions">
                        <button class="icon-btn btn-success" onclick="editItem(${item.id})">✏️</button>
                        <button class="icon-btn btn-danger" onclick="deleteItem(${item.id})">🗑️</button>
                    </div>
                </div>
            `).join('');
        }

        function showAddItem() {
            if (currentUser.role !== 'admin') {
                showToast('Admin access required!', true);
                return;
            }
            
            editingItemId = null;
            document.getElementById('itemModalTitle').textContent = 'Add New Item';
            document.getElementById('itemName').value = '';
            document.getElementById('itemCategory').value = 'meat';
            document.getElementById('itemPrice').value = '';
            document.getElementById('itemStock').value = '';
            document.getElementById('itemImage').value = '';
            document.getElementById('imagePreview').innerHTML = '';
            document.getElementById('itemModal').classList.remove('hidden');
        }

        function editItem(itemId) {
            if (currentUser.role !== 'admin') {
                showToast('Admin access required!', true);
                return;
            }
            
            const item = items.find(i => i.id === itemId);
            if (!item) return;
            
            editingItemId = itemId;
            document.getElementById('itemModalTitle').textContent = 'Edit Item';
            document.getElementById('itemName').value = item.name;
            document.getElementById('itemCategory').value = item.category;
            document.getElementById('itemPrice').value = item.price;
            document.getElementById('itemStock').value = item.stock;
            document.getElementById('itemImage').value = '';
            
            if (item.image) {
                document.getElementById('imagePreview').innerHTML = `<img src="${item.image}" style="max-width:150px;border-radius:8px;">`;
            } else {
                document.getElementById('imagePreview').innerHTML = '';
            }
            
            document.getElementById('itemModal').classList.remove('hidden');
        }

        function previewImage(input) {
            const file = input.files[0];
            if (!file) return;
            
            const reader = new FileReader();
            reader.onload = function(e) {
                document.getElementById('imagePreview').innerHTML = `<img src="${e.target.result}" style="max-width:150px;border-radius:8px;">`;
            };
            reader.readAsDataURL(file);
        }

        function saveItem() {
            const name = document.getElementById('itemName').value.trim();
            const category = document.getElementById('itemCategory').value;
            const price = parseFloat(document.getElementById('itemPrice').value);
            const stock = parseInt(document.getElementById('itemStock').value);
            const fileInput = document.getElementById('itemImage');
            
            if (!name || isNaN(price) || isNaN(stock)) {
                showToast('Please fill all fields correctly!', true);
                return;
            }
            
            let imageData = '';
            if (fileInput.files[0]) {
                const reader = new FileReader();
                reader.onload = function(e) {
                    imageData = e.target.result;
                    performSave(name, category, price, stock, imageData);
                };
                reader.readAsDataURL(fileInput.files[0]);
            } else {
                if (editingItemId) {
                    const existing = items.find(i => i.id === editingItemId);
                    imageData = existing ? existing.image : '';
                }
                performSave(name, category, price, stock, imageData);
            }
        }

        function performSave(name, category, price, stock, image) {
            if (editingItemId) {
                const item = items.find(i => i.id === editingItemId);
                if (item) {
                    item.name = name;
                    item.category = category;
                    item.price = price;
                    item.stock = stock;
                    item.image = image;
                }
            } else {
                const newId = items.length > 0 ? Math.max(...items.map(i => i.id)) + 1 : 1;
                items.push({ id: newId, name, category, price, stock, image });
            }
            
            localStorage.setItem('kb_items', JSON.stringify(items));
            closeItemModal();
            renderItems();
            renderInventory();
            showToast(editingItemId ? 'Item updated!' : 'Item added!');
        }

        function closeItemModal() {
            document.getElementById('itemModal').classList.add('hidden');
            editingItemId = null;
        }

        function deleteItem(itemId) {
            if (currentUser.role !== 'admin') {
                showToast('Admin access required!', true);
                return;
            }
            
            const item = items.find(i => i.id === itemId);
            if (!item) return;
            
            if (!confirm(`Delete "${item.name}"? This cannot be undone.`)) return;
            
            items = items.filter(i => i.id !== itemId);
            localStorage.setItem('kb_items', JSON.stringify(items));
            renderItems();
            renderInventory();
            showToast('Item deleted');
        }

        // ==================== ADMIN AUTHENTICATION ====================
        function verifyAdminAuth() {
            const password = document.getElementById('adminAuthPassword').value;
            const errorDiv = document.getElementById('adminAuthError');
            
            if (password === currentUser.password) {
                document.getElementById('adminAuthModal').classList.add('hidden');
                if (pendingAdminAction) {
                    pendingAdminAction();
                    pendingAdminAction = null;
                }
            } else {
                errorDiv.textContent = 'Incorrect password!';
            }
        }

        function closeAdminAuth() {
            document.getElementById('adminAuthModal').classList.add('hidden');
            pendingAdminAction = null;
        }

        // ==================== STATISTICS & PERFORMANCE GRAPH ====================
        function setGraphPeriod(period) {
            graphPeriod = period;
            document.querySelectorAll('.graph-filter-btn').forEach(btn => btn.classList.remove('active'));
            event.target.classList.add('active');
            renderPerformanceGraph();
        }

        function renderStatistics() {
            const today = new Date().toDateString();
            const todayTrans = transactions.filter(t => new Date(t.date).toDateString() === today && t.status !== 'cancelled');
            
            const totalRevenue = todayTrans.reduce((sum, t) => sum + t.total, 0);
            const totalItems = todayTrans.reduce((sum, t) => sum + t.items.reduce((isum, i) => isum + i.qty, 0), 0);
            const avgOrder = todayTrans.length > 0 ? totalRevenue / todayTrans.length : 0;
            
            document.getElementById('totalRevenue').textContent = totalRevenue.toFixed(0);
            document.getElementById('totalTransactions').textContent = todayTrans.length;
            document.getElementById('itemsSold').textContent = totalItems;
            document.getElementById('avgOrder').textContent = avgOrder.toFixed(0);
            
            // Top selling items
            const itemSales = {};
            transactions.forEach(t => {
                if (t.status !== 'cancelled') {
                    t.items.forEach(i => {
                        if (!itemSales[i.name]) itemSales[i.name] = 0;
                        itemSales[i.name] += i.qty;
                    });
                }
            });
            
            const sortedItems = Object.entries(itemSales).sort((a, b) => b[1] - a[1]).slice(0, 5);
            const maxItemSales = sortedItems.length > 0 ? sortedItems[0][1] : 1;
            
            document.getElementById('topItemsChart').innerHTML = sortedItems.map(([name, qty]) => `
                <div class="chart-bar">
                    <div class="chart-label">${name}</div>
                    <div class="chart-bar-fill">
                        <div class="chart-bar-inner" style="width:${(qty / maxItemSales * 100)}%"></div>
                    </div>
                    <div class="chart-value">${qty}</div>
                </div>
            `).join('') || '<div style="text-align:center;color:#aaa;">No data yet</div>';
            
            // Day of week
            const dayNames = ['Sunday', 'Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday'];
            const daySales = {};
            dayNames.forEach(d => daySales[d] = 0);
            
            transactions.forEach(t => {
                if (t.status !== 'cancelled') {
                    const day = dayNames[new Date(t.date).getDay()];
                    daySales[day] += t.total;
                }
            });
            
            const maxDaySales = Math.max(...Object.values(daySales), 1);
            
            document.getElementById('dayOfWeekChart').innerHTML = dayNames.map(day => `
                <div class="chart-bar">
                    <div class="chart-label">${day.slice(0, 3)}</div>
                    <div class="chart-bar-fill">
                        <div class="chart-bar-inner" style="width:${(daySales[day] / maxDaySales * 100)}%"></div>
                    </div>
                    <div class="chart-value">${daySales[day].toFixed(0)}</div>
                </div>
            `).join('');
            
            // Render performance graph
            renderPerformanceGraph();
        }

        function renderPerformanceGraph() {
            const svg = document.getElementById('performanceGraph');
            const container = document.getElementById('graphContainer');
            const width = container.clientWidth;
            const height = container.clientHeight;
            const padding = { top: 20, right: 30, bottom: 40, left: 50 };
            
            // Get data points based on period
            const data = getGraphData();
            
            if (data.length === 0) {
                svg.innerHTML = `<text x="${width/2}" y="${height/2}" text-anchor="middle" fill="#aaa" font-size="14">No sales data yet</text>`;
                return;
            }
            
            const maxValue = Math.max(...data.map(d => d.value), 1);
            const minValue = 0;
            
            const chartWidth = width - padding.left - padding.right;
            const chartHeight = height - padding.top - padding.bottom;
            
            // Generate SVG content
            let svgContent = '';
            
            // Grid lines
            for (let i = 0; i <= 5; i++) {
                const y = padding.top + (chartHeight * i / 5);
                const value = maxValue * (1 - i / 5);
                svgContent += `<line class="graph-grid-line" x1="${padding.left}" y1="${y}" x2="${width - padding.right}" y2="${y}" />`;
                svgContent += `<text class="graph-axis-text" x="${padding.left - 10}" y="${y + 4}" text-anchor="end">${value.toFixed(0)}</text>`;
            }
            
            // Data points and line
            const points = data.map((d, i) => {
                const x = padding.left + (chartWidth * i / (data.length - 1 || 1));
                const y = padding.top + chartHeight - (chartHeight * (d.value / maxValue));
                return { x, y, label: d.label, value: d.value };
            });
            
            // Area path
            const areaPath = `M ${points[0].x} ${padding.top + chartHeight} ` + 
                points.map(p => `L ${p.x} ${p.y}`).join(' ') + 
                ` L ${points[points.length-1].x} ${padding.top + chartHeight} Z`;
            svgContent += `<path class="graph-area" d="${areaPath}" />`;
            
            // Line path
            const linePath = `M ${points[0].x} ${points[0].y} ` + points.slice(1).map(p => `L ${p.x} ${p.y}`).join(' ');
            svgContent += `<path class="graph-line" d="${linePath}" />`;
            
            // Dots and labels
            points.forEach((p, i) => {
                svgContent += `<circle class="graph-dot" cx="${p.x}" cy="${p.y}" r="5" 
                    onmouseenter="showTooltip(${p.x}, ${p.y}, '${p.label}', ${p.value})" 
                    onmouseleave="hideTooltip()" />`;
                
                // X-axis labels
                const labelX = i === 0 ? p.x + 5 : i === points.length - 1 ? p.x - 5 : p.x;
                svgContent += `<text class="graph-axis-text" x="${labelX}" y="${height - 10}" text-anchor="${i === 0 ? 'start' : i === points.length - 1 ? 'end' : 'middle'}">${p.label}</text>`;
            });
            
            svg.setAttribute('viewBox', `0 0 ${width} ${height}`);
            svg.innerHTML = svgContent;
        }

        function getGraphData() {
            const now = new Date();
            const data = [];
            
            if (graphPeriod === 'week') {
                // Last 7 days
                for (let i = 6; i >= 0; i--) {
                    const date = new Date(now);
                    date.setDate(date.getDate() - i);
                    const dayTrans = transactions.filter(t => {
                        const tDate = new Date(t.date);
                        return tDate.toDateString() === date.toDateString() && t.status !== 'cancelled';
                    });
                    const total = dayTrans.reduce((sum, t) => sum + t.total, 0);
                    data.push({
                        label: date.toLocaleDateString('en', { weekday: 'short' }),
                        value: total
                    });
                }
            } else if (graphPeriod === 'month') {
                // Last 30 days grouped by week
                for (let i = 3; i >= 0; i--) {
                    const endDate = new Date(now);
                    endDate.setDate(endDate.getDate() - (i * 7));
                    const startDate = new Date(endDate);
                    startDate.setDate(startDate.getDate() - 6);
                    
                    const weekTrans = transactions.filter(t => {
                        const tDate = new Date(t.date);
                        return tDate >= startDate && tDate <= endDate && t.status !== 'cancelled';
                    });
                    const total = weekTrans.reduce((sum, t) => sum + t.total, 0);
                    data.push({
                        label: `W${4-i}`,
                        value: total
                    });
                }
            } else if (graphPeriod === 'year') {
                // Last 12 months
                for (let i = 11; i >= 0; i--) {
                    const date = new Date(now.getFullYear(), now.getMonth() - i, 1);
                    const monthTrans = transactions.filter(t => {
                        const tDate = new Date(t.date);
                        return tDate.getMonth() === date.getMonth() && tDate.getFullYear() === date.getFullYear() && t.status !== 'cancelled';
                    });
                    const total = monthTrans.reduce((sum, t) => sum + t.total, 0);
                    data.push({
                        label: date.toLocaleDateString('en', { month: 'short' }),
                        value: total
                    });
                }
            }
            
            return data;
        }

        function showTooltip(x, y, label, value) {
            const tooltip = document.getElementById('graphTooltip');
            tooltip.innerHTML = `<strong>${label}</strong><br>${value.toFixed(2)} ETB`;
            tooltip.style.left = (x + 10) + 'px';
            tooltip.style.top = (y - 40) + 'px';
            tooltip.classList.add('show');
        }

        function hideTooltip() {
            document.getElementById('graphTooltip').classList.remove('show');
        }

        // Handle window resize for graph
        window.addEventListener('resize', () => {
            if (currentUser && currentUser.role === 'admin') {
                renderPerformanceGraph();
            }
        });

        // ==================== UTILITY FUNCTIONS ====================
        function showToast(message, isError = false) {
            const toast = document.getElementById('toast');
            toast.textContent = message;
            toast.className = 'toast' + (isError ? ' error' : '') + ' show';
            
            setTimeout(() => {
                toast.classList.remove('show');
            }, 3000);
        }

        let lastTouchEnd = 0;
        document.addEventListener('touchend', function(event) {
            const now = Date.now();
            if (now - lastTouchEnd <= 300) {
                event.preventDefault();
            }
            lastTouchEnd = now;
        }, false);

        document.addEventListener('keydown', function(e) {
            if (e.key === 'Escape') {
                closeCheckout();
                closeItemModal();
                closeAdminAuth();
            }
        });
    </script>
</body>
</html>
