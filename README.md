<!DOCTYPE html>
<html lang="he" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>בודק מניות ישראליות - עליות שיא</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.1.3/dist/css/bootstrap.min.css" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css">
    <style>
        :root {
            --primary: #2c3e50;
            --secondary: #3498db;
            --success: #27ae60;
            --light-bg: #f8f9fa;
        }
        
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background-color: #f5f7fa;
            color: #333;
            line-height: 1.6;
        }
        
        .hebrew-text {
            font-family: 'Arial Hebrew', Arial, sans-serif;
        }
        
        .header {
            background: linear-gradient(135deg, var(--primary) 0%, var(--secondary) 100%);
            color: white;
            padding: 2rem 0;
            border-radius: 0 0 20px 20px;
            margin-bottom: 2rem;
            box-shadow: 0 4px 12px rgba(0,0,0,0.1);
        }
        
        .card {
            border: none;
            border-radius: 15px;
            box-shadow: 0 5px 15px rgba(0,0,0,0.08);
            transition: transform 0.3s;
            margin-bottom: 1.5rem;
        }
        
        .card:hover {
            transform: translateY(-5px);
        }
        
        .stock-table {
            width: 100%;
            border-collapse: separate;
            border-spacing: 0;
        }
        
        .stock-table th {
            background-color: var(--light-bg);
            color: var(--primary);
            font-weight: 600;
            padding: 1rem;
            border-bottom: 2px solid #dee2e6;
        }
        
        .stock-table td {
            padding: 1rem;
            border-bottom: 1px solid #eee;
        }
        
        .stock-table tr:hover {
            background-color: rgba(52, 152, 219, 0.05);
        }
        
        .positive-change {
            color: var(--success);
            font-weight: 600;
        }
        
        .filters {
            background-color: white;
            padding: 1.5rem;
            border-radius: 15px;
            box-shadow: 0 5px 15px rgba(0,0,0,0.05);
        }
        
        .date-input {
            border: 2px solid #e0e0e0;
            border-radius: 10px;
            padding: 0.75rem;
            width: 100%;
            transition: border 0.3s;
        }
        
        .date-input:focus {
            border-color: var(--secondary);
            outline: none;
        }
        
        .btn-custom {
            background: linear-gradient(to right, var(--secondary), #2980b9);
            color: white;
            border: none;
            border-radius: 10px;
            padding: 0.75rem 2rem;
            font-weight: 600;
            transition: all 0.3s;
        }
        
        .btn-custom:hover {
            transform: translateY(-2px);
            box-shadow: 0 5px 15px rgba(52, 152, 219, 0.3);
            color: white;
        }
        
        .slider-container {
            margin: 1.5rem 0;
        }
        
        .slider-value {
            font-weight: 600;
            color: var(--primary);
        }
        
        .stock-card {
            border-left: 4px solid var(--success);
        }
        
        .stock-card.negative {
            border-left-color: #e74c3c;
        }
        
        @media (max-width: 768px) {
            .header h1 {
                font-size: 1.8rem;
            }
        }
    </style>
</head>
<body>
    <div class="header text-center">
        <div class="container">
            <h1 class="hebrew-text mb-3"><i class="fas fa-chart-line me-2"></i>בודק מניות ישראליות - עליות שיא</h1>
            <p class="lead">נתוני ביצועי מניות בטווחי תאריכים עם אפשרויות סינון מתקדמות</p>
        </div>
    </div>

    <div class="container">
        <div class="row mb-4">
            <div class="col-12">
                <div class="filters">
                    <h4 class="hebrew-text mb-4"><i class="fas fa-filter me-2"></i>סינון וחיפוש</h4>
                    
                    <div class="row">
                        <div class="col-md-3 mb-3">
                            <label for="startDate" class="form-label hebrew-text">תאריך התחלה</label>
                            <input type="date" id="startDate" class="date-input" value="2025-01-01">
                        </div>
                        
                        <div class="col-md-3 mb-3">
                            <label for="endDate" class="form-label hebrew-text">תאריך סיום</label>
                            <input type="date" id="endDate" class="date-input" value="2025-01-20">
                        </div>
                        
                        <div class="col-md-3 mb-3">
                            <label for="numStocks" class="form-label hebrew-text">מספר מניות להצגה</label>
                            <select id="numStocks" class="form-select date-input">
                                <option value="5">5 מניות</option>
                                <option value="10" selected>10 מניות</option>
                                <option value="20">20 מניות</option>
                                <option value="50">50 מניות</option>
                            </select>
                        </div>
                        
                        <div class="col-md-3 mb-3 d-flex align-items-end">
                            <button id="searchBtn" class="btn btn-custom w-100 hebrew-text">
                                <i class="fas fa-search me-2"></i>חפש מניות
                            </button>
                        </div>
                    </div>
                    
                    <div class="row mt-3">
                        <div class="col-md-6">
                            <div class="slider-container">
                                <label class="form-label hebrew-text">
                                    <i class="fas fa-money-bill-wave me-2"></i>
                                    סינון לפי שווי שוק מינימלי (מיליארד ₪)
                                    <span id="marketCapValue" class="slider-value">1</span>
                                </label>
                                <input type="range" id="marketCapFilter" class="form-range" min="0" max="50" step="0.5" value="1">
                                <div class="d-flex justify-content-between">
                                    <small>0 מיליארד</small>
                                    <small>50 מיליארד</small>
                                </div>
                            </div>
                        </div>
                        
                        <div class="col-md-6">
                            <div class="slider-container">
                                <label class="form-label hebrew-text">
                                    <i class="fas fa-exchange-alt me-2"></i>
                                    סינון לפי סחירות מינימלית (מיליון ₪)
                                    <span id="liquidityValue" class="slider-value">5</span>
                                </label>
                                <input type="range" id="liquidityFilter" class="form-range" min="0" max="100" step="5" value="5">
                                <div class="d-flex justify-content-between">
                                    <small>0 מיליון</small>
                                    <small>100 מיליון</small>
                                </div>
                            </div>
                        </div>
                    </div>
                    
                    <div class="row mt-2">
                        <div class="col-12">
                            <div class="form-check form-switch">
                                <input class="form-check-input" type="checkbox" id="onlyPositive" checked>
                                <label class="form-check-label hebrew-text" for="onlyPositive">
                                    הצג רק מניות עם עלייה באחוזים
                                </label>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>

        <div class="row">
            <div class="col-12">
                <div class="card">
                    <div class="card-body">
                        <h5 class="card-title hebrew-text">
                            <i class="fas fa-trophy me-2"></i>
                            המניות המובילות בטווח התאריכים הנבחר
                            <span id="dateRangeText" class="text-muted" style="font-size: 0.9rem;"></span>
                        </h5>
                        
                        <div class="table-responsive">
                            <table class="stock-table">
                                <thead>
                                    <tr>
                                        <th class="hebrew-text">#</th>
                                        <th class="hebrew-text">שם המניה</th>
                                        <th class="hebrew-text">סימול</th>
                                        <th class="hebrew-text">מחיר התחלתי (₪)</th>
                                        <th class="hebrew-text">מחיר סופי (₪)</th>
                                        <th class="hebrew-text">שינוי באחוזים</th>
                                        <th class="hebrew-text">שווי שוק (מיליארד ₪)</th>
                                        <th class="hebrew-text">סחירות יומית ממוצעת (מיליון ₪)</th>
                                    </tr>
                                </thead>
                                <tbody id="stocksTableBody">
                                    <!-- כאן יוטענו הנתונים -->
                                </tbody>
                            </table>
                        </div>
                        
                        <div class="mt-4 text-center" id="noResults" style="display: none;">
                            <div class="alert alert-warning hebrew-text">
                                <i class="fas fa-exclamation-triangle me-2"></i>
                                לא נמצאו מניות העומדות בקריטריונים שבחרת. נסה לשנות את פרמטרי הסינון.
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
        
        <div class="row mt-4">
            <div class="col-md-8">
                <div class="card">
                    <div class="card-body">
                        <h5 class="card-title hebrew-text"><i class="fas fa-info-circle me-2"></i>כיצד המערכת עובדת</h5>
                        <ul class="hebrew-text">
                            <li>המערכת מאחזרת נתוני מניות ישראליות בטווח התאריכים שבחרת</li>
                            <li>מחושבת העלייה/ירידה באחוזים בין התאריכים</li>
                            <li>המניות ממוינות לפי הביצועים הטובים ביותר</li>
                            <li>ניתן לסנן לפי שווי שוק מינימלי וסחירות מינימלית</li>
                            <li>הנתונים מתעדכנים אוטומטית עם כל שינוי בסינון</li>
                        </ul>
                        <div class="alert alert-info mt-3 hebrew-text">
                            <strong>הערה:</strong> גרסה זו מציגה נתוני הדגמה. לחיבור לנתונים אמיתיים, יש לחבר למערכת API פיננסית כמו זו של הבורסה לניירות ערך בתל אביב.
                        </div>
                    </div>
                </div>
            </div>
            
            <div class="col-md-4">
                <div class="card">
                    <div class="card-body">
                        <h5 class="card-title hebrew-text"><i class="fas fa-chart-pie me-2"></i>סטטיסטיקות</h5>
                        <div class="hebrew-text">
                            <p><strong>מספר מניות בסינון:</strong> <span id="statsCount">0</span></p>
                            <p><strong>שווי שוק ממוצע:</strong> <span id="statsAvgMarketCap">0</span> מיליארד ₪</p>
                            <p><strong>עלייה ממוצעת:</strong> <span id="statsAvgChange">0</span>%</p>
                            <p><strong>טווח תאריכים:</strong> <span id="statsDateRange">-</span></p>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <footer class="mt-5 py-4 text-center text-muted hebrew-text" style="background-color: var(--light-bg);">
        <div class="container">
            <p>מערכת בדיקת מניות ישראליות | למידע פיננסי מעודכן יש לפנות למקורות רשמיים</p>
            <p class="mb-0">© 2025 כל הזכויות שמורות</p>
        </div>
    </footer>

    <script>
        // נתוני מניות לדוגמה (במציאות יהיו מטעינת API)
        const sampleStocks = [
            { name: "תכלית סל", symbol: "TASE:TTLT", startPrice: 45.2, endPrice: 58.7, marketCap: 12.5, avgLiquidity: 45.2 },
            { name: "בזק", symbol: "TASE:BEZQ", startPrice: 23.1, endPrice: 32.4, marketCap: 8.3, avgLiquidity: 120.5 },
            { name: "טבע", symbol: "TASE:TEVA", startPrice: 12.8, endPrice: 18.3, marketCap: 25.7, avgLiquidity: 85.3 },
            { name: "אלביט", symbol: "TASE:ELBT", startPrice: 245.5, endPrice: 312.8, marketCap: 42.1, avgLiquidity: 65.7 },
            { name: "אי.די.אי", symbol: "TASE:IDEB", startPrice: 78.9, endPrice: 102.4, marketCap: 15.8, avgLiquidity: 32.1 },
            { name: "מגדל", symbol: "TASE:MGDL", startPrice: 15.3, endPrice: 21.7, marketCap: 5.2, avgLiquidity: 18.9 },
            { name: "פלאפון", symbol: "TASE:PLPM", startPrice: 32.4, endPrice: 41.6, marketCap: 7.9, avgLiquidity: 40.2 },
            { name: "ישראכרט", symbol: "TASE:ISRA", startPrice: 56.7, endPrice: 67.3, marketCap: 9.4, avgLiquidity: 25.8 },
            { name: "כיל", symbol: "TASE:ICL", startPrice: 18.9, endPrice: 25.4, marketCap: 30.5, avgLiquidity: 55.1 },
            { name: "מאיר שטחים", symbol: "TASE:MSHR", startPrice: 120.5, endPrice: 145.8, marketCap: 3.8, avgLiquidity: 12.4 },
            { name: "הראל", symbol: "TASE:HARL", startPrice: 42.3, endPrice: 51.9, marketCap: 11.2, avgLiquidity: 38.7 },
            { name: "מליסרון", symbol: "TASE:MLSR", startPrice: 87.6, endPrice: 95.2, marketCap: 6.5, avgLiquidity: 15.3 },
            { name: "פז", symbol: "TASE:PZOL", startPrice: 2100, endPrice: 2350, marketCap: 18.9, avgLiquidity: 72.4 },
            { name: "אסם", symbol: "TASE:ASIM", startPrice: 6700, endPrice: 7200, marketCap: 22.4, avgLiquidity: 45.8 },
            { name: "דלק", symbol: "TASE:DLEKG", startPrice: 430, endPrice: 510, marketCap: 35.7, avgLiquidity: 92.1 },
            { name: "אינטל", symbol: "NASDAQ:INTC", startPrice: 32.4, endPrice: 28.7, marketCap: 28.5, avgLiquidity: 150.2 },
            { name: "פרדיס", symbol: "TASE:PRDS", startPrice: 1450, endPrice: 1320, marketCap: 4.3, avgLiquidity: 8.9 },
            { name: "שנקר", symbol: "TASE:SNCM", startPrice: 890, endPrice: 920, marketCap: 2.1, avgLiquidity: 5.4 },
            { name: "מבני תעשיה", symbol: "TASE:MBTZ", startPrice: 320, endPrice: 410, marketCap: 1.8, avgLiquidity: 7.2 },
            { name: "קסם", symbol: "TASE:KSEM", startPrice: 5600, endPrice: 6100, marketCap: 9.7, avgLiquidity: 22.5 }
        ];

        // אלמנטים ב-DOM
        const startDateEl = document.getElementById('startDate');
        const endDateEl = document.getElementById('endDate');
        const numStocksEl = document.getElementById('numStocks');
        const searchBtn = document.getElementById('searchBtn');
        const marketCapFilter = document.getElementById('marketCapFilter');
        const liquidityFilter = document.getElementById('liquidityFilter');
        const marketCapValue = document.getElementById('marketCapValue');
        const liquidityValue = document.getElementById('liquidityValue');
        const onlyPositiveEl = document.getElementById('onlyPositive');
        const stocksTableBody = document.getElementById('stocksTableBody');
        const dateRangeText = document.getElementById('dateRangeText');
        const noResultsEl = document.getElementById('noResults');
        const statsCount = document.getElementById('statsCount');
        const statsAvgMarketCap = document.getElementById('statsAvgMarketCap');
        const statsAvgChange = document.getElementById('statsAvgChange');
        const statsDateRange = document.getElementById('statsDateRange');

        // הגדרת תאריכים ברירת מחדל
        const today = new Date();
        const oneMonthAgo = new Date();
        oneMonthAgo.setMonth(today.getMonth() - 1);
        
        // פורמט תאריך ל-YYYY-MM-DD
        function formatDate(date) {
            return date.toISOString().split('T')[0];
        }
        
        // אם אין תאריכים שהוזנו, הגדר ברירת מחדל
        if (!startDateEl.value) startDateEl.value = formatDate(oneMonthAgo);
        if (!endDateEl.value) endDateEl.value = formatDate(today);
        
        // עדכון ערכי הסליידרים
        marketCapValue.textContent = marketCapFilter.value;
        liquidityValue.textContent = liquidityFilter.value;
        
        // האזנה לאירועים
        marketCapFilter.addEventListener('input', () => {
            marketCapValue.textContent = marketCapFilter.value;
            updateStocksTable();
        });
        
        liquidityFilter.addEventListener('input', () => {
            liquidityValue.textContent = liquidityFilter.value;
            updateStocksTable();
        });
        
        searchBtn.addEventListener('click', updateStocksTable);
        onlyPositiveEl.addEventListener('change', updateStocksTable);
        
        // פונקציה לחישוב אחוז שינוי
        function calculateChange(start, end) {
            return ((end - start) / start * 100).toFixed(2);
        }
        
        // פונקציה לעדכון הטבלה
        function updateStocksTable() {
            const startDate = startDateEl.value;
            const endDate = endDateEl.value;
            const numStocks = parseInt(numStocksEl.value);
            const minMarketCap = parseFloat(marketCapFilter.value);
            const minLiquidity = parseFloat(liquidityFilter.value);
            const onlyPositive = onlyPositiveEl.checked;
            
            // עדכון טקסט טווח התאריכים
            dateRangeText.textContent = `(${startDate} עד ${endDate})`;
            statsDateRange.textContent = `${startDate} - ${endDate}`;
            
            // חישוב נתונים עבור כל מניה
            const stocksData = sampleStocks.map(stock => {
                const change = calculateChange(stock.startPrice, stock.endPrice);
                return {
                    ...stock,
                    change: parseFloat(change),
                    absChange: Math.abs(parseFloat(change))
                };
            });
            
            // סינון המניות לפי הקריטריונים
            let filteredStocks = stocksData.filter(stock => {
                if (minMarketCap > 0 && stock.marketCap < minMarketCap) return false;
                if (minLiquidity > 0 && stock.avgLiquidity < minLiquidity) return false;
                if (onlyPositive && stock.change <= 0) return false;
                return true;
            });
            
            // מיון לפי הביצועים הטובים ביותר (אחוז שינוי גבוה ביותר)
            filteredStocks.sort((a, b) => b.change - a.change);
            
            // הגבלה למספר המניות המבוקש
            filteredStocks = filteredStocks.slice(0, numStocks);
            
            // הצגת ההודעה אם אין תוצאות
            if (filteredStocks.length === 0) {
                noResultsEl.style.display = 'block';
                stocksTableBody.innerHTML = '';
            } else {
                noResultsEl.style.display = 'none';
                
                // בניית שורות הטבלה
                let tableHTML = '';
                let totalChange = 0;
                let totalMarketCap = 0;
                
                filteredStocks.forEach((stock, index) => {
                    totalChange += stock.change;
                    totalMarketCap += stock.marketCap;
                    
                    tableHTML += `
                        <tr>
                            <td class="hebrew-text">${index + 1}</td>
                            <td class="hebrew-text"><strong>${stock.name}</strong></td>
                            <td class="text-muted">${stock.symbol}</td>
                            <td>${stock.startPrice.toLocaleString()}</td>
                            <td>${stock.endPrice.toLocaleString()}</td>
                            <td class="${stock.change >= 0 ? 'positive-change' : 'text-danger'}">
                                <i class="fas ${stock.change >= 0 ? 'fa-arrow-up' : 'fa-arrow-down'} me-1"></i>
                                ${stock.change >= 0 ? '+' : ''}${stock.change}%
                            </td>
                            <td>${stock.marketCap.toLocaleString()}</td>
                            <td>${stock.avgLiquidity.toLocaleString()}</td>
                        </tr>
                    `;
                });
                
                stocksTableBody.innerHTML = tableHTML;
                
                // עדכון סטטיסטיקות
                statsCount.textContent = filteredStocks.length;
                statsAvgMarketCap.textContent = (totalMarketCap / filteredStocks.length).toFixed(1);
                statsAvgChange.textContent = (totalChange / filteredStocks.length).toFixed(2);
            }
        }
        
        // אתחול ראשוני
        updateStocksTable();
        
        // פונקציה לדוגמה להתחברות ל-API אמיתי
        async function fetchRealStockData(startDate, endDate) {
            // כאן יתווסף הקוד להתחברות ל-API פיננסי אמיתי
            console.log(`מבקש נתונים מ-API עבור ${startDate} עד ${endDate}`);
            
            // הדגמה בלבד - במציאות כאן יהיה קריאה ל-API
            return new Promise(resolve => {
                setTimeout(() => {
                    resolve(sampleStocks);
                }, 500);
            });
        }
        
        // אפשרות להתחברות ל-API אמיתי
        document.addEventListener('DOMContentLoaded', function() {
            console.log("האתר מוכן לשימוש! לחיבור ל-API אמיתי, יש לשנות את הפונקציה fetchRealStockData.");
        });
    </script>
</body>
</html>
