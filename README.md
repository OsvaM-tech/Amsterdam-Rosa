<html lang="it">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Amsterdam Weekend - Rosa</title>
    <link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" />
    <script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Arial, sans-serif;
            background: #f5f5f5;
            color: #333;
            line-height: 1.6;
        }

        .header {
            background: linear-gradient(135deg, #ff6b35 0%, #f7931e 100%);
            color: white;
            padding: 30px 20px;
            text-align: center;
            box-shadow: 0 2px 10px rgba(0,0,0,0.1);
        }

        .header h1 {
            font-size: 28px;
            margin-bottom: 5px;
        }

        .nav-tabs {
            display: flex;
            background: white;
            box-shadow: 0 2px 5px rgba(0,0,0,0.1);
            position: sticky;
            top: 0;
            z-index: 100;
            overflow-x: auto;
        }

        .nav-tab {
            flex: 1;
            min-width: 80px;
            padding: 15px 10px;
            text-align: center;
            cursor: pointer;
            border: none;
            background: white;
            color: #666;
            font-weight: 500;
            border-bottom: 3px solid transparent;
            transition: all 0.3s;
        }

        .nav-tab.active {
            color: #ff6b35;
            border-bottom-color: #ff6b35;
            background: #fff8f5;
        }

        .content {
            display: none;
            padding: 20px;
            max-width: 800px;
            margin: 0 auto;
        }

        .content.active {
            display: block;
        }

        .day-header {
            display: flex;
            align-items: center;
            margin-bottom: 20px;
            padding: 15px;
            background: linear-gradient(135deg, #ff6b35 0%, #f7931e 100%);
            color: white;
            border-radius: 12px;
        }

        .day-number {
            width: 50px;
            height: 50px;
            background: white;
            color: #ff6b35;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 24px;
            font-weight: bold;
            margin-right: 15px;
        }

        .activity-card {
            background: white;
            border-radius: 12px;
            padding: 20px;
            margin-bottom: 15px;
            box-shadow: 0 2px 8px rgba(0,0,0,0.08);
            transition: all 0.3s;
            cursor: pointer;
        }

        .activity-card:hover {
            transform: translateY(-2px);
            box-shadow: 0 4px 20px rgba(0,0,0,0.12);
        }

        .time-badge {
            display: inline-block;
            background: #e3f2fd;
            color: #1e3c72;
            padding: 5px 12px;
            border-radius: 20px;
            font-size: 14px;
            font-weight: 500;
            margin-bottom: 10px;
        }

        .activity-title {
            font-size: 18px;
            font-weight: 600;
            margin-bottom: 8px;
        }

        .activity-description {
            color: #666;
            margin-bottom: 10px;
        }

        .meta-item {
            display: inline-block;
            margin-right: 15px;
            color: #999;
            font-size: 14px;
        }

        .travel-card {
            background: white;
            border-radius: 12px;
            padding: 20px;
            margin-bottom: 20px;
            box-shadow: 0 2px 8px rgba(0,0,0,0.08);
        }

        .travel-card h3 {
            color: #ff6b35;
            margin-bottom: 15px;
        }

        .flight-route {
            display: flex;
            align-items: center;
            justify-content: space-around;
            padding: 20px;
            background: #f8f9fa;
            border-radius: 8px;
            margin-bottom: 15px;
        }

        .airport {
            text-align: center;
        }

        .airport-code {
            font-size: 28px;
            font-weight: bold;
            color: #1e3c72;
        }

        .airport-time {
            font-size: 20px;
            color: #666;
            margin-top: 5px;
        }

        .flight-arrow {
            font-size: 30px;
            color: #ff6b35;
        }

        .info-card {
            background: white;
            border-radius: 12px;
            padding: 20px;
            margin-bottom: 20px;
            box-shadow: 0 2px 8px rgba(0,0,0,0.08);
        }

        .info-card h3 {
            color: #ff6b35;
            margin-bottom: 15px;
        }

        .info-list {
            list-style: none;
        }

        .info-list li {
            padding: 10px 0;
            border-bottom: 1px solid #f0f0f0;
        }

        .fab {
            position: fixed;
            bottom: 20px;
            right: 20px;
            width: 60px;
            height: 60px;
            background: #ff6b35;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            color: white;
            font-size: 24px;
            box-shadow: 0 4px 20px rgba(0,0,0,0.3);
            cursor: pointer;
            z-index: 100;
        }

        .modal {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0,0,0,0.5);
            z-index: 2000;
        }

        .modal-content {
            position: absolute;
            bottom: 0;
            left: 0;
            right: 0;
            background: white;
            border-radius: 20px 20px 0 0;
            padding: 30px 20px;
            max-height: 80vh;
            overflow-y: auto;
        }

        .modal-close {
            position: absolute;
            top: 20px;
            right: 20px;
            width: 30px;
            height: 30px;
            background: #f0f0f0;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            font-size: 20px;
            color: #666;
        }

        .modal-close:hover {
            background: #e0e0e0;
        }

        .map-container {
            height: 400px;
            width: 100%;
            position: relative;
            border-radius: 12px;
            overflow: hidden;
        }

        #map {
            height: 100%;
            width: 100%;
        }

        /* Custom marker styles */
        .marker-21 {
            background: #ff6b35;
            color: white;
            width: 30px;
            height: 30px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-weight: bold;
            font-size: 12px;
            border: 3px solid white;
            box-shadow: 0 2px 5px rgba(0,0,0,0.3);
        }

        .marker-23 {
            background: #1e3c72;
            color: white;
            width: 30px;
            height: 30px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-weight: bold;
            font-size: 12px;
            border: 3px solid white;
            box-shadow: 0 2px 5px rgba(0,0,0,0.3);
        }

        .marker-24 {
            background: #8e44ad;
            color: white;
            width: 30px;
            height: 30px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-weight: bold;
            font-size: 12px;
            border: 3px solid white;
            box-shadow: 0 2px 5px rgba(0,0,0,0.3);
        }

        .gps-btn {
            position: absolute;
            top: 10px;
            right: 10px;
            z-index: 1000;
            background: white;
            border: 2px solid rgba(0,0,0,0.2);
            border-radius: 4px;
            padding: 8px;
            cursor: pointer;
            box-shadow: 0 1px 5px rgba(0,0,0,0.4);
        }

        .gps-btn:hover {
            background: #f4f4f4;
        }

        .gps-btn.active {
            background: #4285f4;
            color: white;
        }

        @media (max-width: 768px) {
            .flight-route {
                flex-direction: column;
            }
            .flight-arrow {
                transform: rotate(90deg);
                margin: 10px 0;
            }
        }
    </style>
</head>
<body>
    <div class="header">
        <h1>🌷 Amsterdam Weekend</h1>
        <p>Viaggio di Rosa - 21-24 Giugno</p>
    </div>

    <div class="nav-tabs">
        <button class="nav-tab active" onclick="showTab('mappa')">📍 Mappa</button>
        <button class="nav-tab" onclick="showTab('giorno21')">21 Giugno</button>
        <button class="nav-tab" onclick="showTab('giorno23')">23 Giugno</button>
        <button class="nav-tab" onclick="showTab('giorno24')">24 Giugno</button>
        <button class="nav-tab" onclick="showTab('viaggio')">✈️ Viaggio</button>
        <button class="nav-tab" onclick="showTab('info')">ℹ️ Info</button>
    </div>

    <!-- Mappa -->
    <div id="mappa" class="content active">
        <div class="map-container">
            <div id="map"></div>
            <button class="gps-btn" onclick="toggleGPS()" title="La mia posizione">📍</button>
        </div>
    </div>

    <!-- Giorno 21 -->
    <div id="giorno21" class="content">
        <div class="day-header">
            <div class="day-number">21</div>
            <div>
                <h2>Venerdì 21 Giugno</h2>
                <p>Arrivo e Centro Storico</p>
            </div>
        </div>

        <div class="activity-card">
            <div class="time-badge">07:45 - 09:00</div>
            <h3 class="activity-title">✈️ Arrivo e trasferimento</h3>
            <p class="activity-description">Volo EasyJet U2 3851 (06:00-07:45). Treno per Amsterdam Centraal e deposito bagaglio in stazione.</p>
            <div>
                <span class="meta-item">🚊 Treno aeroporto €5.50</span>
                <span class="meta-item">🧳 Deposito bagagli eventuale (Stasher) €7-9</span>
            </div>
        </div>

	<div class="activity-card" onclick="showOnMap(52.3738, 4.8910, 'Piazza Dam')">
 		<div class="time-badge">09:30 - 11:00</div>
            <h3 class="activity-title">Piazza Dam e dintorni</h3>
            <p class="activity-description">Esplora il cuore di Amsterdam. Opzioni: Palazzo Reale (€12.50) o Nieuwe Kerk. Colazione da De Bijenkorf.</p>
            <div>
                <span class="meta-item">📍 Dam Square</span>
                <span class="meta-item">☕ Café nei dintorni</span>
            </div>
        </div>

        <div class="activity-card" onclick="showOnMap(52.3747, 4.9124, 'Bloemenmarkt')">
            <div class="time-badge">11:00 - 12:00</div>
            <h3 class="activity-title">Bloemenmarkt</h3>
            <p class="activity-description">Il mercato dei fiori galleggiante sul canale Singel. Perfetto per le foto!</p>
            <div>
                <span class="meta-item">📍 Singel</span>
                <span class="meta-item">🌷 Ingresso gratuito</span>
            </div>
        </div>

        <div class="activity-card" onclick="showOnMap(52.3672, 4.8938, 'Begijnhof')">
            <div class="time-badge">12:00 - 13:00</div>
            <h3 class="activity-title">Begijnhof</h3>
            <p class="activity-description">Cortile medievale nascosto nel centro. Oasi di pace con case storiche e cappella.</p>
            <div>
                <span class="meta-item">📍 Ingresso da Spui</span>
                <span class="meta-item">🆓 Gratuito</span>
            </div>
        </div>

        <div class="activity-card" onclick="showOnMap(52.3802, 4.8999, 'Traghetto gratuito')">
            <div class="time-badge">13:00 - 14:00</div>
            <h3 class="activity-title">Mini Crociera Gratuita</h3>
            <p class="activity-description">Traghetto gratuito GVB dal retro della Stazione Centrale verso Noord. Vista panoramica della città dall'acqua.</p>
            <div>
                <span class="meta-item">🚢 Traghetto F3 gratuito</span>
                <span class="meta-item">⏱️ Ogni 5 minuti</span>
            </div>
        </div>

        <div class="activity-card">
            <div class="time-badge">14:30</div>
            <h3 class="activity-title">🚊 Partenza per Nijmegen</h3>
            <p class="activity-description">Recupera il bagaglio e prendi il treno da Amsterdam Centraal. Prenota su NS.nl o app 9292.</p>
            <div>
                <span class="meta-item">⏱️ Durata: 1h 20min</span>
                <span class="meta-item">💰 €20-25</span>
            </div>
        </div>
    </div>

    <!-- Giorno 23 -->
    <div id="giorno23" class="content">
        <div class="day-header">
            <div class="day-number">23</div>
            <div>
                <h2>Lunedì 23 Giugno</h2>
                <p>Quartieri e Movida</p>
            </div>
        </div>

        <div class="activity-card">
            <div class="time-badge">09:00 - 10:20</div>
            <h3 class="activity-title">🚊 Ritorno da Nijmegen</h3>
            <p class="activity-description">Treno con l'amica. Sfruttate il NS Groepsticket per risparmiare il 40% sul secondo biglietto!</p>
            <div>
                <span class="meta-item">💰 €20-25 + €12-15 (sconto gruppo)</span>
                <span class="meta-item">📍 Arrivo: Amsterdam Centraal</span>
            </div>
        </div>

        <div class="activity-card" onclick="showOnMap(52.3660, 4.8852, 'Jordaan')">
            <div class="time-badge">11:00 - 13:30</div>
            <h3 class="activity-title">Quartiere Jordaan</h3>
            <p class="activity-description">Il quartiere più caratteristico: canali, mercatini, café brown. Pranzo da Winkel 43 (apple pie famosa).</p>
            <div>
                <span class="meta-item">📍 Jordaan</span>
                <span class="meta-item">🥧 Winkel 43</span>
            </div>
        </div>

        <div class="activity-card" onclick="showOnMap(52.3499, 4.8901, 'De Pijp')">
            <div class="time-badge">14:00 - 17:00</div>
            <h3 class="activity-title">De Pijp - Quartiere Latino</h3>
            <p class="activity-description">Zona trendy e multiculturale. Albert Cuyp Market (mercato più grande), boutique vintage, café hipster.</p>
            <div>
                <span class="meta-item">📍 De Pijp</span>
                <span class="meta-item">🛍️ Albert Cuypmarkt</span>
            </div>
        </div>

        <div class="activity-card" onclick="showOnMap(52.3629, 4.8810, 'Leidseplein')">
            <div class="time-badge">17:30 - 19:30</div>
            <h3 class="activity-title">Aperitivo a Leidseplein</h3>
            <p class="activity-description">Piazza vivace con bar e locali. Happy hour e primo drink della serata.</p>
            <div>
                <span class="meta-item">📍 Leidseplein</span>
                <span class="meta-item">🍻 Café Americain</span>
            </div>
        </div>

        <div class="activity-card" onclick="showOnMap(52.3499, 4.8901, 'De Pijp')">
            <div class="time-badge">20:00 - 23:00</div>
            <h3 class="activity-title">Cena e Serata a De Pijp</h3>
            <p class="activity-description">Torna a De Pijp per cena. Zona piena di ristoranti etnici e locali notturni. Dopo cena: bar e club.</p>
            <div>
                <span class="meta-item">🍽️ Bakers & Roasters</span>
                <span class="meta-item">🎉 Twenty Third Bar</span>
            </div>
        </div>
    </div>

    <!-- Giorno 24 -->
    <div id="giorno24" class="content">
        <div class="day-header">
            <div class="day-number">24</div>
            <div>
                <h2>Martedì 24 Giugno</h2>
                <p>Relax e Partenza</p>
            </div>
        </div>

        <div class="activity-card" onclick="showOnMap(52.3584, 4.8810, 'Zona Musei')">
            <div class="time-badge">10:00 - 12:00</div>
            <h3 class="activity-title">Zona Musei (opzionale)</h3>
            <p class="activity-description">Se avete energia: Van Gogh Museum (€20) o Rijksmuseum (€22.50). Altrimenti passeggiata nella zona.</p>
            <div>
                <span class="meta-item">📍 Museumplein</span>
                <span class="meta-item">💡 Prenota online per saltare code</span>
            </div>
        </div>

        <div class="activity-card" onclick="showOnMap(52.3579, 4.8686, 'Vondelpark')">
            <div class="time-badge">12:00 - 14:00</div>
            <h3 class="activity-title">Vondelpark</h3>
            <p class="activity-description">Il polmone verde di Amsterdam. Relax, picnic o pranzo al Groot Melkhuis nel parco.</p>
            <div>
                <span class="meta-item">📍 Vondelpark</span>
                <span class="meta-item">☕ Groot Melkhuis</span>
            </div>
        </div>

        <div class="activity-card" onclick="showOnMap(52.3738, 4.8910, 'Centro')">
            <div class="time-badge">14:30 - 16:00</div>
            <h3 class="activity-title">Ultimo giro in centro</h3>
            <p class="activity-description">Shopping last minute, souvenir. Recupera bagagli se lasciati in deposito.</p>
            <div>
                <span class="meta-item">📍 Centro/Dam</span>
                <span class="meta-item">🛍️ Kalverstraat</span>
            </div>
        </div>

        <div class="activity-card">
            <div class="time-badge">16:30</div>
            <h3 class="activity-title">🚊 Partenza per l'aeroporto</h3>
            <p class="activity-description">Treno da Centraal a Schiphol. Arriva in aeroporto almeno 2 ore prima del volo (17:10).</p>
            <div>
                <span class="meta-item">⏱️ 15 minuti di treno</span>
                <span class="meta-item">💰 €5.50</span>
            </div>
        </div>

        <div class="activity-card">
            <div class="time-badge">19:10</div>
            <h3 class="activity-title">✈️ Volo per Milano</h3>
            <p class="activity-description">Volo EasyJet U2 3858 per Milano Malpensa (19:10-20:55).</p>
            <div>
                <span class="meta-item">🎫 Gate chiude 30 min prima</span>
                <span class="meta-item">🚌 Transfer Flibco alle 23:30</span>
            </div>
        </div>
    </div>

    <!-- Viaggio -->
    <div id="viaggio" class="content">
        <div class="travel-card">
            <h3>✈️ Volo Andata - Venerdì 21 Giugno</h3>
            <div class="flight-route">
                <div class="airport">
                    <div class="airport-code">MXP</div>
                    <div class="airport-time">06:00</div>
                    <div style="font-size: 12px; color: #999;">Milano Malpensa</div>
                </div>
                <div class="flight-arrow">→</div>
                <div class="airport">
                    <div class="airport-code">AMS</div>
                    <div class="airport-time">07:45</div>
                    <div style="font-size: 12px; color: #999;">Amsterdam Schiphol</div>
                </div>
            </div>
            <div style="text-align: center; color: #666;">
                <p>Volo EasyJet U2 3851 • Durata: 1h 45min</p>
            </div>
        </div>

        <div class="travel-card">
            <h3>🚌 Transfer Andata: Torino → Malpensa</h3>
            <p><strong>Azienda:</strong> Flibco</p>
            <p><strong>Partenza:</strong> Torino - ore 02:00 circa</p>
            <p><strong>Arrivo:</strong> Terminal 2 Malpensa - ore 03:30 circa</p>
            <p style="margin-top: 10px; color: #666;">⚠️ Verifica orario esatto sul sito Flibco</p>
        </div>

        <div class="travel-card">
            <h3>✈️ Volo Ritorno - Martedì 24 Giugno</h3>
            <div class="flight-route">
                <div class="airport">
                    <div class="airport-code">AMS</div>
                    <div class="airport-time">19:10</div>
                    <div style="font-size: 12px; color: #999;">Amsterdam Schiphol</div>
                </div>
                <div class="flight-arrow">→</div>
                <div class="airport">
                    <div class="airport-code">MXP</div>
                    <div class="airport-time">20:55</div>
                    <div style="font-size: 12px; color: #999;">Milano Malpensa</div>
                </div>
            </div>
            <div style="text-align: center; color: #666;">
                <p>Volo EasyJet U2 3858 • Durata: 1h 45min</p>
            </div>
        </div>

        <div class="travel-card">
            <h3>🚌 Transfer Ritorno: Malpensa → Torino</h3>
            <p><strong>Azienda:</strong> Flibco</p>
            <p><strong>Partenza:</strong> Terminal 2 Malpensa - ore 23:30 circa</p>
            <p><strong>Arrivo:</strong> Torino - ore 01:00 circa</p>
            <p style="margin-top: 10px; color: #666;">⚠️ Prenota in anticipo su flibco.com</p>
        </div>
    </div>

    <!-- Info -->
    <div id="info" class="content">
        <div class="info-card">
            <h3>🚊 Trasporti</h3>
            <ul class="info-list">
                <li>GVB Day Ticket Amsterdam: €8.50</li>
                <li>Noleggio bici: €10-15/giorno</li>
                <li>Treno aeroporto Schiphol: €5.50</li>
                <li>App 9292: per pianificare tutti gli spostamenti in Olanda</li>
                <li><strong>Treno Amsterdam-Nijmegen:</strong> €20-25 singolo</li>
                <li>💡 <strong>Risparmio gruppo:</strong> -40% dal 2° passeggero</li>
            </ul>
        </div>

        <div class="info-card">
            <h3>🍽️ Dove Mangiare</h3>
            <ul class="info-list">
                <li>Winkel 43 - Apple pie leggendaria</li>
                <li>Café de Reiger - Cucina olandese</li>
                <li>Bakers & Roasters - Brunch</li>
                <li>Groot Melkhuis - Nel Vondelpark</li>
            </ul>
        </div>

        <div class="info-card">
            <h3>💡 Consigli</h3>
            <ul class="info-list">
		    <li>Manda baci ad Osvaldo che ti ama tanto</li>
		    <li>Per i locali usa la Lonely planet</li>
		    <li>Guarda i percorsi della guida per girare i quartieri</li>
                <li>Prenota musei online</li>
                <li>Fai un giretto in bici bici</li>
            </ul>
        </div>
    </div>

    <div class="fab" onclick="toggleChecklist()">📋</div>

    <div id="modal" class="modal" onclick="closeModal(event)">
        <div class="modal-content">
            <span class="modal-close" onclick="closeModal()">×</span>
            <h2 style="color: #ff6b35; margin-bottom: 20px;">Checklist Viaggio</h2>
            <label style="display: block; margin: 10px 0;">
                <input type="checkbox"> Carta d'identità e passaporto
            </label>
            <label style="display: block; margin: 10px 0;">
                <input type="checkbox"> Prenotazione Flibco
            </label>
            <label style="display: block; margin: 10px 0;">
                <input type="checkbox"> Check-in EasyJet
            </label>
		<label style="display: block; margin: 10px 0;">
                <input type="checkbox"> Lonely planet (in particolare locali e percorsi)
            </label>
            <label style="display: block; margin: 10px 0;">
                <input type="checkbox"> Prenotazioni musei
            </label>
            <label style="display: block; margin: 10px 0;">
                <input type="checkbox"> Contanti
            </label>
            <label style="display: block; margin: 10px 0;">
                <input type="checkbox"> App 9292
            </label>
        </div>
    </div>

    <script>
        let map = null;
        let markers = [];
        let userMarker = null;
        let watchId = null;

        // Locations for the map
        const locations = [
            // Day 21
            {lat: 52.3738, lng: 4.8910, name: 'Piazza Dam', day: 21},
            {lat: 52.3747, lng: 4.9124, name: 'Bloemenmarkt', day: 21},
            {lat: 52.3672, lng: 4.8938, name: 'Begijnhof', day: 21},
            {lat: 52.3802, lng: 4.8999, name: 'Traghetto gratuito', day: 21},
            // Day 23
            {lat: 52.3660, lng: 4.8852, name: 'Jordaan', day: 23},
            {lat: 52.3499, lng: 4.8901, name: 'De Pijp', day: 23},
            {lat: 52.3629, lng: 4.8810, name: 'Leidseplein', day: 23},
            // Day 24
            {lat: 52.3584, lng: 4.8810, name: 'Zona Musei', day: 24},
            {lat: 52.3579, lng: 4.8686, name: 'Vondelpark', day: 24}
        ];

        function initMap() {
            if (!map && typeof L !== 'undefined') {
                map = L.map('map').setView([52.3702, 4.8952], 13);
                
                L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
                    attribution: '© OpenStreetMap'
                }).addTo(map);

                // Add markers
                locations.forEach(loc => {
                    const icon = L.divIcon({
                        className: 'custom-div-icon',
                        html: `<div class="marker-${loc.day}">${loc.day}</div>`,
                        iconSize: [30, 30],
                        iconAnchor: [15, 15]
                    });

                    const marker = L.marker([loc.lat, loc.lng], {icon: icon})
                        .addTo(map)
                        .bindPopup(`<strong>${loc.name}</strong><br>Giorno ${loc.day}`);
                    
                    markers.push(marker);
                });
            }
        }

        function showTab(tabName) {
            // Remove active class from all tabs
            document.querySelectorAll('.nav-tab').forEach(tab => {
                tab.classList.remove('active');
            });
            
            // Add active class to clicked tab
            event.target.classList.add('active');
            
            // Hide all content
            document.querySelectorAll('.content').forEach(content => {
                content.classList.remove('active');
            });
            
            // Show selected content
            document.getElementById(tabName).classList.add('active');

            // Initialize map if showing map tab
            if (tabName === 'mappa') {
                setTimeout(initMap, 100);
            }
        }

        function showOnMap(lat, lng, name) {
            // Switch to map tab
            document.querySelectorAll('.nav-tab')[0].click();
            
            // Wait for tab switch and map initialization
            setTimeout(() => {
                if (map) {
                    map.setView([lat, lng], 16);
                    
                    // Find and open the popup for the clicked location
                    markers.forEach(marker => {
                        const markerPos = marker.getLatLng();
                        if (Math.abs(markerPos.lat - lat) < 0.0001 && Math.abs(markerPos.lng - lng) < 0.0001) {
                            marker.openPopup();
                        }
                    });
                }
            }, 300);
        }

        function toggleGPS() {
            const btn = document.querySelector('.gps-btn');
            
            if (watchId) {
                navigator.geolocation.clearWatch(watchId);
                watchId = null;
                btn.classList.remove('active');
                if (userMarker) {
                    map.removeLayer(userMarker);
                    userMarker = null;
                }
            } else {
                if (navigator.geolocation) {
                    btn.classList.add('active');
                    
                    navigator.geolocation.getCurrentPosition(pos => {
                        const {latitude, longitude} = pos.coords;
                        
                        if (userMarker) {
                            userMarker.setLatLng([latitude, longitude]);
                        } else {
                            userMarker = L.circleMarker([latitude, longitude], {
                                radius: 8,
                                fillColor: "#4285f4",
                                color: "#fff",
                                weight: 3,
                                opacity: 1,
                                fillOpacity: 1
                            }).addTo(map);
                        }
                        
                        map.setView([latitude, longitude], 15);
                    });
                    
                    watchId = navigator.geolocation.watchPosition(pos => {
                        const {latitude, longitude} = pos.coords;
                        if (userMarker) {
                            userMarker.setLatLng([latitude, longitude]);
                        }
                    });
                } else {
                    alert('Geolocalizzazione non supportata');
                }
            }
        }

        function toggleChecklist() {
            const modal = document.getElementById('modal');
            modal.style.display = modal.style.display === 'block' ? 'none' : 'block';
        }

        function closeModal(event) {
            if (!event || event.target.className === 'modal' || event.target.className === 'modal-close') {
                document.getElementById('modal').style.display = 'none';
            }
        }

        // Save checkbox states
        document.addEventListener('DOMContentLoaded', function() {
            const checkboxes = document.querySelectorAll('input[type="checkbox"]');
            
            // Load saved states
            checkboxes.forEach((checkbox, index) => {
                const saved = localStorage.getItem('check_' + index);
                if (saved === 'true') {
                    checkbox.checked = true;
                }
                
                // Save on change
                checkbox.addEventListener('change', function() {
                    localStorage.setItem('check_' + index, this.checked);
                });
            });

            // Initialize map on load if map tab is active
            if (document.getElementById('mappa').classList.contains('active')) {
                setTimeout(initMap, 500);
            }
        });
    </script>
</body>
</html>
