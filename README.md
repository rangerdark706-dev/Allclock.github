# Allclock.github
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>World Time Finder</title>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@300;400;600;800&family=JetBrains+Mono:wght@400;700&display=swap');

        :root {
            --primary: #00f2ff;
            --secondary: #bd00ff;
            --glass-bg: rgba(20, 20, 30, 0.65);
            --glass-border: rgba(255, 255, 255, 0.1);
            --text-main: #ffffff;
            --text-muted: #a0a0b0;
        }

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
        }

        body {
            font-family: 'Inter', sans-serif;
            background-color: #050505;
            background-image: url('https://image.qwenlm.ai/public_source/9f6bc5b0-6e80-4fbf-86ea-bcb2f8e51ee6/1a1930e54-c309-4280-b99c-a6463b4dc635.png');
            background-size: cover;
            background-position: center;
            background-attachment: fixed;
            color: var(--text-main);
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            overflow-x: hidden;
        }

        /* Overlay to darken background slightly for readability */
        body::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0, 0, 0, 0.4);
            z-index: -1;
        }

        .container {
            width: 90%;
            max-width: 500px;
            background: var(--glass-bg);
            backdrop-filter: blur(20px);
            -webkit-backdrop-filter: blur(20px);
            border: 1px solid var(--glass-border);
            border-radius: 24px;
            padding: 2rem;
            box-shadow: 0 20px 50px rgba(0, 0, 0, 0.5);
            animation: fadeIn 0.8s ease-out;
            position: relative;
            z-index: 1;
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(20px); }
            to { opacity: 1; transform: translateY(0); }
        }

        header {
            text-align: center;
            margin-bottom: 2rem;
        }

        h1 {
            font-weight: 800;
            font-size: 1.8rem;
            background: linear-gradient(135deg, var(--primary), var(--secondary));
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            margin-bottom: 0.5rem;
            letter-spacing: -1px;
        }

        p.subtitle {
            color: var(--text-muted);
            font-size: 0.9rem;
        }

        .search-box {
            position: relative;
            margin-bottom: 2rem;
        }

        input[type="text"] {
            width: 100%;
            padding: 1rem 1.5rem;
            border-radius: 12px;
            border: 1px solid var(--glass-border);
            background: rgba(0, 0, 0, 0.3);
            color: white;
            font-family: 'Inter', sans-serif;
            font-size: 1rem;
            outline: none;
            transition: all 0.3s ease;
        }

        input[type="text"]:focus {
            border-color: var(--primary);
            box-shadow: 0 0 15px rgba(0, 242, 255, 0.2);
            background: rgba(0, 0, 0, 0.5);
        }

        .suggestions {
            position: absolute;
            top: 100%;
            left: 0;
            width: 100%;
            background: #1a1a25;
            border: 1px solid var(--glass-border);
            border-radius: 12px;
            margin-top: 0.5rem;
            max-height: 200px;
            overflow-y: auto;
            z-index: 10;
            display: none;
            box-shadow: 0 10px 30px rgba(0,0,0,0.5);
        }

        .suggestions.active {
            display: block;
        }

        .suggestion-item {
            padding: 0.8rem 1.5rem;
            cursor: pointer;
            border-bottom: 1px solid rgba(255,255,255,0.05);
            transition: background 0.2s;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        .suggestion-item:last-child {
            border-bottom: none;
        }

        .suggestion-item:hover {
            background: rgba(255, 255, 255, 0.1);
        }

        .suggestion-item span.flag {
            margin-right: 10px;
            font-size: 1.2rem;
        }

        .time-display {
            text-align: center;
            padding: 2rem 0;
            border-top: 1px solid var(--glass-border);
            border-bottom: 1px solid var(--glass-border);
            margin: 1.5rem 0;
            position: relative;
        }

        .clock {
            font-family: 'JetBrains Mono', monospace;
            font-size: 3.5rem;
            font-weight: 700;
            color: var(--primary);
            text-shadow: 0 0 20px rgba(0, 242, 255, 0.4);
            margin-bottom: 0.5rem;
            letter-spacing: -2px;
        }

        .date {
            font-size: 1.1rem;
            color: var(--text-muted);
            font-weight: 400;
        }

        .country-info {
            text-align: center;
        }

        .country-name {
            font-size: 1.5rem;
            font-weight: 600;
            margin-bottom: 0.2rem;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 10px;
        }

        .timezone-label {
            font-size: 0.85rem;
            color: var(--secondary);
            background: rgba(189, 0, 255, 0.1);
            padding: 4px 12px;
            border-radius: 20px;
            display: inline-block;
            margin-top: 0.5rem;
        }

        .empty-state {
            text-align: center;
            padding: 3rem 0;
            color: var(--text-muted);
        }

        .empty-state svg {
            width: 64px;
            height: 64px;
            margin-bottom: 1rem;
            opacity: 0.5;
        }

        /* Scrollbar styling */
        ::-webkit-scrollbar {
            width: 8px;
        }
        ::-webkit-scrollbar-track {
            background: rgba(0,0,0,0.1);
        }
        ::-webkit-scrollbar-thumb {
            background: rgba(255,255,255,0.2);
            border-radius: 4px;
        }
        ::-webkit-scrollbar-thumb:hover {
            background: rgba(255,255,255,0.3);
        }

        @media (max-width: 480px) {
            .container {
                width: 95%;
                padding: 1.5rem;
            }
            .clock {
                font-size: 2.5rem;
            }
        }
    </style>
</head>
<body>

    <div class="container">
        <header>
            <h1>World Time</h1>
            <p class="subtitle">Find the exact time anywhere on Earth</p>
        </header>

        <div class="search-box">
            <input type="text" id="searchInput" placeholder="Search for a country (e.g., Japan, USA)..." autocomplete="off">
            <div class="suggestions" id="suggestionsList"></div>
        </div>

        <div id="resultArea">
            <div class="empty-state">
                <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round">
                    <circle cx="12" cy="12" r="10"></circle>
                    <polyline points="12 6 12 12 16 14"></polyline>
                </svg>
                <p>Select a country to see the time</p>
            </div>
        </div>
    </div>

    <script>
        // Comprehensive list of countries and their primary timezones
        const countries = [
            { name: "United States (New York)", timezone: "America/New_York", flag: "🇺🇸" },
            { name: "United States (Los Angeles)", timezone: "America/Los_Angeles", flag: "🇺🇸" },
            { name: "United Kingdom", timezone: "Europe/London", flag: "🇬🇧" },
            { name: "France", timezone: "Europe/Paris", flag: "🇫🇷" },
            { name: "Germany", timezone: "Europe/Berlin", flag: "🇩🇪" },
            { name: "Japan", timezone: "Asia/Tokyo", flag: "🇯🇵" },
            { name: "China", timezone: "Asia/Shanghai", flag: "🇨🇳" },
            { name: "India", timezone: "Asia/Kolkata", flag: "🇮🇳" },
            { name: "Australia (Sydney)", timezone: "Australia/Sydney", flag: "🇦🇺" },
            { name: "Australia (Perth)", timezone: "Australia/Perth", flag: "🇦🇺" },
            { name: "Brazil (Sao Paulo)", timezone: "America/Sao_Paulo", flag: "🇧🇷" },
            { name: "Canada (Toronto)", timezone: "America/Toronto", flag: "🇨🇦" },
            { name: "Canada (Vancouver)", timezone: "America/Vancouver", flag: "🇨🇦" },
            { name: "Russia (Moscow)", timezone: "Europe/Moscow", flag: "🇷🇺" },
            { name: "South Korea", timezone: "Asia/Seoul", flag: "🇰🇷" },
            { name: "Italy", timezone: "Europe/Rome", flag: "🇮🇹" },
            { name: "Spain", timezone: "Europe/Madrid", flag: "🇪🇸" },
            { name: "Mexico", timezone: "America/Mexico_City", flag: "🇲🇽" },
            { name: "Indonesia (Jakarta)", timezone: "Asia/Jakarta", flag: "🇮🇩" },
            { name: "Turkey", timezone: "Europe/Istanbul", flag: "🇹🇷" },
            { name: "Saudi Arabia", timezone: "Asia/Riyadh", flag: "🇸🇦" },
            { name: "Argentina", timezone: "America/Argentina/Buenos_Aires", flag: "🇦🇷" },
            { name: "South Africa", timezone: "Africa/Johannesburg", flag: "🇿🇦" },
            { name: "Egypt", timezone: "Africa/Cairo", flag: "🇪🇬" },
            { name: "Nigeria", timezone: "Africa/Lagos", flag: "🇳🇬" },
            { name: "Thailand", timezone: "Asia/Bangkok", flag: "🇹🇭" },
            { name: "Vietnam", timezone: "Asia/Ho_Chi_Minh", flag: "🇻🇳" },
            { name: "Philippines", timezone: "Asia/Manila", flag: "🇵🇭" },
            { name: "Malaysia", timezone: "Asia/Kuala_Lumpur", flag: "🇲🇾" },
            { name: "Singapore", timezone: "Asia/Singapore", flag: "🇸🇬" },
            { name: "United Arab Emirates", timezone: "Asia/Dubai", flag: "🇦🇪" },
            { name: "Israel", timezone: "Asia/Jerusalem", flag: "🇮🇱" },
            { name: "New Zealand", timezone: "Pacific/Auckland", flag: "🇳🇿" },
            { name: "Sweden", timezone: "Europe/Stockholm", flag: "🇸🇪" },
            { name: "Norway", timezone: "Europe/Oslo", flag: "🇳🇴" },
            { name: "Denmark", timezone: "Europe/Copenhagen", flag: "🇩🇰" },
            { name: "Finland", timezone: "Europe/Helsinki", flag: "🇫🇮" },
            { name: "Poland", timezone: "Europe/Warsaw", flag: "🇵🇱" },
            { name: "Netherlands", timezone: "Europe/Amsterdam", flag: "🇳🇱" },
            { name: "Belgium", timezone: "Europe/Brussels", flag: "🇧🇪" },
            { name: "Switzerland", timezone: "Europe/Zurich", flag: "🇨🇭" },
            { name: "Austria", timezone: "Europe/Vienna", flag: "🇦🇹" },
            { name: "Portugal", timezone: "Europe/Lisbon", flag: "🇵🇹" },
            { name: "Greece", timezone: "Europe/Athens", flag: "🇬🇷" },
            { name: "Ukraine", timezone: "Europe/Kiev", flag: "🇺🇦" },
            { name: "Pakistan", timezone: "Asia/Karachi", flag: "🇵🇰" },
            { name: "Bangladesh", timezone: "Asia/Dhaka", flag: "🇧🇩" },
            { name: "Iran", timezone: "Asia/Tehran", flag: "🇮🇷" },
            { name: "Iraq", timezone: "Asia/Baghdad", flag: "🇮🇶" },
            { name: "Kenya", timezone: "Africa/Nairobi", flag: "🇰🇪" },
            { name: "Chile", timezone: "America/Santiago", flag: "🇨" },
            { name: "Colombia", timezone: "America/Bogota", flag: "🇨🇴" },
            { name: "Peru", timezone: "America/Lima", flag: "🇵🇪" },
            { name: "Venezuela", timezone: "America/Caracas", flag: "🇻🇪" },
            { name: "Cuba", timezone: "America/Havana", flag: "🇨🇺" },
            { name: "Jamaica", timezone: "America/Jamaica", flag: "🇯🇲" },
            { name: "Panama", timezone: "America/Panama", flag: "🇵🇦" },
            { name: "Costa Rica", timezone: "America/Costa_Rica", flag: "🇨🇷" },
            { name: "Guatemala", timezone: "America/Guatemala", flag: "🇬🇹" },
            { name: "Honduras", timezone: "America/Tegucigalpa", flag: "🇭🇳" },
            { name: "El Salvador", timezone: "America/El_Salvador", flag: "🇸🇻" },
            { name: "Nicaragua", timezone: "America/Managua", flag: "🇳🇮" },
            { name: "Ecuador", timezone: "America/Guayaquil", flag: "🇪🇨" },
            { name: "Bolivia", timezone: "America/La_Paz", flag: "🇧🇴" },
            { name: "Paraguay", timezone: "America/Asuncion", flag: "🇵🇾" },
            { name: "Uruguay", timezone: "America/Montevideo", flag: "🇺🇾" },
            { name: "Iceland", timezone: "Atlantic/Reykjavik", flag: "🇮🇸" },
            { name: "Ireland", timezone: "Europe/Dublin", flag: "🇮🇪" },
            { name: "Czech Republic", timezone: "Europe/Prague", flag: "🇨🇿" },
            { name: "Hungary", timezone: "Europe/Budapest", flag: "🇭🇺" },
            { name: "Romania", timezone: "Europe/Bucharest", flag: "🇷🇴" },
            { name: "Bulgaria", timezone: "Europe/Sofia", flag: "🇧🇬" },
            { name: "Croatia", timezone: "Europe/Zagreb", flag: "🇭🇷" },
            { name: "Serbia", timezone: "Europe/Belgrade", flag: "🇷🇸" },
            { name: "Slovakia", timezone: "Europe/Bratislava", flag: "🇸🇰" },
            { name: "Slovenia", timezone: "Europe/Ljubljana", flag: "🇸🇮" },
            { name: "Estonia", timezone: "Europe/Tallinn", flag: "🇪🇪" },
            { name: "Latvia", timezone: "Europe/Riga", flag: "🇱🇻" },
            { name: "Lithuania", timezone: "Europe/Vilnius", flag: "🇱🇹" },
            { name: "Belarus", timezone: "Europe/Minsk", flag: "🇧🇾" },
            { name: "Moldova", timezone: "Europe/Chisinau", flag: "🇲🇩" },
            { name: "Albania", timezone: "Europe/Tirane", flag: "🇦🇱" },
            { name: "Bosnia and Herzegovina", timezone: "Europe/Sarajevo", flag: "🇧" },
            { name: "North Macedonia", timezone: "Europe/Skopje", flag: "🇲🇰" },
            { name: "Montenegro", timezone: "Europe/Podgorica", flag: "🇲🇪" },
            { name: "Kosovo", timezone: "Europe/Belgrade", flag: "🇽🇰" },
            { name: "Luxembourg", timezone: "Europe/Luxembourg", flag: "🇱🇺" },
            { name: "Malta", timezone: "Europe/Malta", flag: "🇲🇹" },
            { name: "Cyprus", timezone: "Asia/Nicosia", flag: "🇨" },
            { name: "Georgia", timezone: "Asia/Tbilisi", flag: "🇬🇪" },
            { name: "Armenia", timezone: "Asia/Yerevan", flag: "🇦🇲" },
            { name: "Azerbaijan", timezone: "Asia/Baku", flag: "🇦🇿" },
            { name: "Kazakhstan", timezone: "Asia/Almaty", flag: "🇰🇿" },
            { name: "Uzbekistan", timezone: "Asia/Tashkent", flag: "🇺🇿" },
            { name: "Turkmenistan", timezone: "Asia/Ashgabat", flag: "🇹🇲" },
            { name: "Kyrgyzstan", timezone: "Asia/Bishkek", flag: "🇰🇬" },
            { name: "Tajikistan", timezone: "Asia/Dushanbe", flag: "🇹🇯" },
            { name: "Afghanistan", timezone: "Asia/Kabul", flag: "🇦🇫" },
            { name: "Nepal", timezone: "Asia/Kathmandu", flag: "🇳🇵" },
            { name: "Sri Lanka", timezone: "Asia/Colombo", flag: "🇱🇰" },
            { name: "Maldives", timezone: "Indian/Maldives", flag: "🇲🇻" },
            { name: "Bhutan", timezone: "Asia/Thimphu", flag: "🇧🇹" },
            { name: "Myanmar", timezone: "Asia/Yangon", flag: "🇲🇲" },
            { name: "Cambodia", timezone: "Asia/Phnom_Penh", flag: "🇰🇭" },
            { name: "Laos", timezone: "Asia/Vientiane", flag: "🇱🇦" },
            { name: "Mongolia", timezone: "Asia/Ulaanbaatar", flag: "🇲🇳" },
            { name: "North Korea", timezone: "Asia/Pyongyang", flag: "🇰🇵" },
            { name: "Taiwan", timezone: "Asia/Taipei", flag: "🇹🇼" },
            { name: "Hong Kong", timezone: "Asia/Hong_Kong", flag: "🇭🇰" },
            { name: "Macau", timezone: "Asia/Macau", flag: "🇲🇴" },
            { name: "Brunei", timezone: "Asia/Brunei", flag: "🇧🇳" },
            { name: "East Timor", timezone: "Asia/Dili", flag: "🇹🇱" },
            { name: "Papua New Guinea", timezone: "Pacific/Port_Moresby", flag: "🇵🇬" },
            { name: "Fiji", timezone: "Pacific/Fiji", flag: "🇫🇯" },
            { name: "Samoa", timezone: "Pacific/Apia", flag: "🇼🇸" },
            { name: "Tonga", timezone: "Pacific/Tongatapu", flag: "🇹🇴" },
            { name: "Vanuatu", timezone: "Pacific/Efate", flag: "🇻🇺" },
            { name: "Solomon Islands", timezone: "Pacific/Guadalcanal", flag: "🇸🇧" },
            { name: "New Caledonia", timezone: "Pacific/Noumea", flag: "🇳🇨" },
            { name: "French Polynesia", timezone: "Pacific/Tahiti", flag: "🇵🇫" },
            { name: "Cook Islands", timezone: "Pacific/Rarotonga", flag: "🇨🇰" },
            { name: "Kiribati", timezone: "Pacific/Tarawa", flag: "🇰🇮" },
            { name: "Marshall Islands", timezone: "Pacific/Majuro", flag: "🇲🇭" },
            { name: "Micronesia", timezone: "Pacific/Chuuk", flag: "🇫🇲" },
            { name: "Palau", timezone: "Pacific/Palau", flag: "🇵🇼" },
            { name: "Nauru", timezone: "Pacific/Nauru", flag: "🇳🇷" },
            { name: "Tuvalu", timezone: "Pacific/Funafuti", flag: "🇹🇻" },
            { name: "Greenland", timezone: "America/Godthab", flag: "🇬🇱" },
            { name: "Faroe Islands", timezone: "Atlantic/Faroe", flag: "🇫🇴" },
            { name: "Svalbard", timezone: "Arctic/Longyearbyen", flag: "🇸🇯" },
            { name: "Andorra", timezone: "Europe/Andorra", flag: "🇦🇩" },
            { name: "Monaco", timezone: "Europe/Monaco", flag: "🇲🇨" },
            { name: "San Marino", timezone: "Europe/San_Marino", flag: "🇸🇲" },
            { name: "Vatican City", timezone: "Europe/Vatican", flag: "🇻🇦" },
            { name: "Liechtenstein", timezone: "Europe/Vaduz", flag: "🇱🇮" },
            { name: "Gibraltar", timezone: "Europe/Gibraltar", flag: "🇬🇮" },
            { name: "Jersey", timezone: "Europe/Jersey", flag: "🇯🇪" },
            { name: "Guernsey", timezone: "Europe/Guernsey", flag: "🇬🇬" },
            { name: "Isle of Man", timezone: "Europe/Isle_of_Man", flag: "🇮🇲" },
            { name: "Morocco", timezone: "Africa/Casablanca", flag: "🇲🇦" },
            { name: "Algeria", timezone: "Africa/Algiers", flag: "🇩🇿" },
            { name: "Tunisia", timezone: "Africa/Tunis", flag: "🇹🇳" },
            { name: "Libya", timezone: "Africa/Tripoli", flag: "🇱🇾" },
            { name: "Sudan", timezone: "Africa/Khartoum", flag: "🇸🇩" },
            { name: "Ethiopia", timezone: "Africa/Addis_Ababa", flag: "🇪🇹" },
            { name: "Somalia", timezone: "Africa/Mogadishu", flag: "🇸🇴" },
            { name: "Djibouti", timezone: "Africa/Djibouti", flag: "🇩🇯" },
            { name: "Eritrea", timezone: "Africa/Asmara", flag: "🇪🇷" },
            { name: "Tanzania", timezone: "Africa/Dar_es_Salaam", flag: "🇹🇿" },
            { name: "Uganda", timezone: "Africa/Kampala", flag: "🇺🇬" },
            { name: "Rwanda", timezone: "Africa/Kigali", flag: "🇷🇼" },
            { name: "Burundi", timezone: "Africa/Bujumbura", flag: "🇧🇮" },
            { name: "Democratic Republic of the Congo", timezone: "Africa/Kinshasa", flag: "🇨🇩" },
            { name: "Republic of the Congo", timezone: "Africa/Brazzaville", flag: "🇨🇬" },
            { name: "Central African Republic", timezone: "Africa/Bangui", flag: "🇨🇫" },
            { name: "Chad", timezone: "Africa/Ndjamena", flag: "🇹🇩" },
            { name: "Cameroon", timezone: "Africa/Douala", flag: "🇨🇲" },
            { name: "Equatorial Guinea", timezone: "Africa/Malabo", flag: "🇬🇶" },
            { name: "Gabon", timezone: "Africa/Libreville", flag: "🇬🇦" },
            { name: "Sao Tome and Principe", timezone: "Africa/Sao_Tome", flag: "🇸🇹" },
            { name: "Angola", timezone: "Africa/Luanda", flag: "🇦🇴" },
            { name: "Zambia", timezone: "Africa/Lusaka", flag: "🇿🇲" },
            { name: "Zimbabwe", timezone: "Africa/Harare", flag: "🇿🇼" },
            { name: "Botswana", timezone: "Africa/Gaborone", flag: "🇧🇼" },
            { name: "Namibia", timezone: "Africa/Windhoek", flag: "🇳🇦" },
            { name: "Lesotho", timezone: "Africa/Maseru", flag: "🇱🇸" },
            { name: "Eswatini", timezone: "Africa/Mbabane", flag: "🇸🇿" },
            { name: "Mozambique", timezone: "Africa/Maputo", flag: "🇲🇿" },
            { name: "Madagascar", timezone: "Indian/Antananarivo", flag: "🇲🇬" },
            { name: "Mauritius", timezone: "Indian/Mauritius", flag: "🇲🇺" },
            { name: "Seychelles", timezone: "Indian/Mahe", flag: "🇸🇨" },
            { name: "Comoros", timezone: "Indian/Comoro", flag: "🇰🇲" },
            { name: "Mayotte", timezone: "Indian/Mayotte", flag: "🇾🇹" },
            { name: "Reunion", timezone: "Indian/Reunion", flag: "🇷🇪" },
            { name: "Western Sahara", timezone: "Africa/El_Aaiun", flag: "🇪🇭" },
            { name: "Mali", timezone: "Africa/Bamako", flag: "🇲🇱" },
            { name: "Niger", timezone: "Africa/Niamey", flag: "🇳🇪" },
            { name: "Burkina Faso", timezone: "Africa/Ouagadougou", flag: "🇧🇫" },
            { name: "Ivory Coast", timezone: "Africa/Abidjan", flag: "🇨🇮" },
            { name: "Ghana", timezone: "Africa/Accra", flag: "🇬🇭" },
            { name: "Togo", timezone: "Africa/Lome", flag: "🇹🇬" },
            { name: "Benin", timezone: "Africa/Porto-Novo", flag: "🇧🇯" },
            { name: "Guinea", timezone: "Africa/Conakry", flag: "🇬🇳" },
            { name: "Guinea-Bissau", timezone: "Africa/Bissau", flag: "🇬🇼" },
            { name: "Sierra Leone", timezone: "Africa/Freetown", flag: "🇸🇱" },
            { name: "Liberia", timezone: "Africa/Monrovia", flag: "🇱🇷" },
            { name: "Senegal", timezone: "Africa/Dakar", flag: "🇸🇳" },
            { name: "Gambia", timezone: "Africa/Banjul", flag: "🇬🇲" },
            { name: "Cape Verde", timezone: "Atlantic/Cape_Verde", flag: "🇨🇻" },
            { name: "Mauritania", timezone: "Africa/Nouakchott", flag: "🇲🇷" },
            { name: "Oman", timezone: "Asia/Muscat", flag: "🇴🇲" },
            { name: "Yemen", timezone: "Asia/Aden", flag: "🇾🇪" },
            { name: "Kuwait", timezone: "Asia/Kuwait", flag: "🇰🇼" },
            { name: "Bahrain", timezone: "Asia/Bahrain", flag: "🇧🇭" },
            { name: "Qatar", timezone: "Asia/Qatar", flag: "🇶🇦" },
            { name: "Jordan", timezone: "Asia/Amman", flag: "🇯🇴" },
            { name: "Lebanon", timezone: "Asia/Beirut", flag: "🇱🇧" },
            { name: "Syria", timezone: "Asia/Damascus", flag: "🇸🇾" },
            { name: "Palestine", timezone: "Asia/Gaza", flag: "🇵🇸" },
            { name: "Laos", timezone: "Asia/Vientiane", flag: "🇱🇦" }
        ];

        const searchInput = document.getElementById('searchInput');
        const suggestionsList = document.getElementById('suggestionsList');
        const resultArea = document.getElementById('resultArea');
        
        let selectedTimezone = null;
        let timerInterval = null;

        // Initialize with user's local time
        const localTimezone = Intl.DateTimeFormat().resolvedOptions().timeZone;
        displayTime(localTimezone, "Your Location", "🌍");

        searchInput.addEventListener('input', (e) => {
            const query = e.target.value.toLowerCase();
            suggestionsList.innerHTML = '';
            
            if (query.length === 0) {
                suggestionsList.classList.remove('active');
                return;
            }

            const filtered = countries.filter(c => c.name.toLowerCase().includes(query));
            
            if (filtered.length > 0) {
                suggestionsList.classList.add('active');
                filtered.forEach(country => {
                    const div = document.createElement('div');
                    div.className = 'suggestion-item';
                    div.innerHTML = `<span>${country.flag} ${country.name}</span>`;
                    div.addEventListener('click', () => {
                        selectCountry(country);
                        searchInput.value = '';
                        suggestionsList.classList.remove('active');
                        searchInput.blur();
                    });
                    suggestionsList.appendChild(div);
                });
            } else {
                suggestionsList.classList.remove('active');
            }
        });

        // Close suggestions when clicking outside
        document.addEventListener('click', (e) => {
            if (!searchInput.contains(e.target) && !suggestionsList.contains(e.target)) {
                suggestionsList.classList.remove('active');
            }
        });

        function selectCountry(country) {
            selectedTimezone = country.timezone;
            displayTime(country.timezone, country.name, country.flag);
        }

        function displayTime(timezone, name, flag) {
            resultArea.innerHTML = `
                <div class="country-info">
                    <div class="country-name">
                        <span style="font-size: 1.8rem;">${flag}</span>
                        ${name}
                    </div>
                    <div class="timezone-label">${timezone}</div>
                </div>
                <div class="time-display">
                    <div class="clock" id="clockDisplay">--:--:--</div>
                    <div class="date" id="dateDisplay">Loading...</div>
                </div>
            `;
            
            if (timerInterval) clearInterval(timerInterval);
            updateClock(timezone);
            timerInterval = setInterval(() => updateClock(timezone), 1000);
        }

        function updateClock(timezone) {
            const now = new Date();
            
            const timeOptions = {
                timeZone: timezone,
                hour: '2-digit',
                minute: '2-digit',
                second: '2-digit',
                hour12: false
            };
            
            const dateOptions = {
                timeZone: timezone,
                weekday: 'long',
                year: 'numeric',
                month: 'long',
                day: 'numeric'
            };

            const timeString = new Intl.DateTimeFormat('en-US', timeOptions).format(now);
            const dateString = new Intl.DateTimeFormat('en-US', dateOptions).format(now);

            const clockEl = document.getElementById('clockDisplay');
            const dateEl = document.getElementById('dateDisplay');

            if (clockEl) clockEl.textContent = timeString;
            if (dateEl) dateEl.textContent = dateString;
        }
    </script>
</body>
</html>

