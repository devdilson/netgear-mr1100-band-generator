# netgear-mr5200-band-generator


## Show signal strength

[Credits to MioNonno](https://www.youtube.com/c/MioNonno).

```javascript
javascript: addBar();

function gInfo() {
    $.ajax({
        type: "GET",
        async: !0,
        url: "/api/model.json?internalapi=1",
        error: function(n, a, r) {
            console.log("Signal Error:" + n.status + "\nmessage:" + n.responseText + "\nerror:" + r)
        },
        success: function(n) {
            for (resp = JSON.parse(n), devTemp = resp.general.devTemperature, battTemp = resp.power.batteryTemperature, battChargeSource = resp.power.battChargeSource, charging = resp.power.charging, wwan = resp.wwan, sessDuration = Math.floor(wwan.sessDuration / 60), SCCcount = wwan.ca.SCCcount, connectionText = wwan.connectionText, dataTransferred = wwan.dataUsage.generic.dataTransferred, battChargeLevel = resp.power.battChargeLevel, total = String((dataTransferred / 1073741824).toFixed(2)) + " GB", signalStrength = wwan.signalStrength, diagInfo = wwan.diagInfo, rsrp = signalStrength.rsrp, rsrq = signalStrength.rsrq, sinr = signalStrength.sinr, rssi = signalStrength.rssi, nr5gBandInfo = wwan.nr5gBandInfo, wwanadv = resp.wwanadv, radioQuality = wwanadv.radioQuality, curBand = wwanadv.curBand, cellId = wwanadv.cellId, mcc = wwanadv.MCC, mnc = wwanadv.MNC, plmn = mcc + mnc, IP = wwan.IP, vars = ["rssi", "rsrp", "rsrq", "sinr", "radioQuality", "curBand", "cellId", "plmn", "SCCcount", "sessDuration", "connectionText", "total", "battChargeLevel", "battChargeSource", "IP", "devTemp", "battTemp"], i = 0; i < vars.length; i++) $("#" + vars[i]).html(window[vars[i]]);
            if (setgraph("rsrp", rsrp, -130, -70), setgraph("rsrq", rsrq, -16, -3), e5 = "undefined" != typeof nr5gBandInfo, e5)
                for (nr5rsrp = diagInfo[0].nr5gsigRsrp.replace(" dBm", ""), nr5rsrq = diagInfo[0].nr5gsigRsrq.replace(" dB", ""), nr5sinr = diagInfo[0].nr5gsigSnr, setgraph("nr5rsrp", nr5rsrp, -130, -70), setgraph("nr5rsrq", nr5rsrq, -16, -3), get5GBands(), vars = ["nr5rsrp", "nr5rsrq", "nr5sinr"], i = 0; i < vars.length; i++) $("#" + vars[i]).html(window[vars[i]]);
            $(".e5").toggle(e5), get4GBands(), madebymiononno = 1, hex = Number(cellId).toString(16), hex2 = hex.substring(0, hex.length - 2), enbid = parseInt(hex2, 16).toString(), $("#enbid").html(enbid), "22201" == plmn && (plmn = "2221"), "22299" == plmn && (plmn = "22288"), "22250" == plmn && 6 == enbid.length && (plmn = "22288"), link_lte = "https://lteitaly.it/internal/map.php#bts=" + plmn + "." + enbid, $("#lteitaly").attr("href", link_lte)
        }
    })
}

function get5GBands() {
    for (var n = wwan.nr5gBandInfo, a = "", r = 0; r <= n.length; r++) void 0 !== n[r] && 0 != Object.keys(n[r]).length && (tc = n[r], tb = tc.band, tw = tc.dlBandwidth, tc.isPcc ? a += "<strong>N" + tb + "</strong>" : a += "N" + tb, a += "(" + tw + ") +");
    $("#nr5gbands").html(a.slice(0, -1))
}

function get4GBands() {
    var n = wwan.lteBandInfo;
    if (void 0 !== n) {
        $(".field4g").show();
        for (var a = "", r = 0; r <= n.length; r++) void 0 !== n[r] && 0 != Object.keys(n[r]).length && (tc = n[r], "-1" != tc.channel && (tb = tc.band, tw = tc.dlBandwidth, tc.isPcc ? a += "<strong>B" + tb + "</strong>" : a += "B" + tb, a += "(" + tw + ") +"));
        $("#ltebands").html(a.slice(0, -1))
    }
}

function setgraph(n, a, r, i) {
    wwx = (a - r) / (i - r) * 100, xs = String(wwx) + String.fromCharCode(37), e = "#" + n + "b", $(e).animate({
        width: xs
    }), $(e).html(n + " : " + window[n]), wwx < 50 ? $(e).css("background-color", "yellow").css("color", "black") : (85 < wwx ? $(e).css("background-color", "orange") : $(e).css("background-color", "green")).css("color", "white")
}

function addBar() {
    $("body").prepend('<style> #rsrq,#nr5rsrp, #rsrp,#nr5rsrq, #rssi, #enbid, #sinr,#nr5sinr, #cellId, #band, #radioQuality, #curband, #nr5gbands, #ltebands, #devTemp, #battTemp, #IP, #battChargeSource, #battChargeLevel, #total, #sessDuration, #SCCcount {color: #b00; font-weight: strong; } .field4g, .field5g {display: none; } .f {float: left; border: 1px solid #bbb; border-radius: 5px; padding: 10px; line-height: 2em; margin: 5px; } .f ul {margin: 0; padding: 0; } .f ul li {display: inline; margin-right: 10px; } #mode {margin-right: 0 !important; } #enbid {font-weight: bold; text-decoration: underline; } .p {border-bottom: 1px solid #ccc; width: auto; height: 20px; } .v {height: 100%25; border-right: 1px solid #aaa; } .sb {padding: 10px; border-radius: 10px; display: inline-block; margin: 10px 0 10px 10px; } #t {color: white; background-color: #888; margin: 10px; padding: 25px; border-radius: 10px; display: none; text-align: center; font-weight: bolder; } .v {padding-left: 20px; } </style> <div class="p e5"> <div class="v" id="nr5rsrpb"></div> </div> <div class="p e5"> <div class="v" id="nr5rsrqb"></div> </div> <div class="p"> <div class="v" id="rsrpb"></div> </div> <div class="p"> <div class="v" id="rsrqb"></div> </div> <div style="display:block;overflow: auto;"> <div class="f"> <ul> <li><a style="font-weight:bolder;background-color: #448;color:white;padding: 10px;border-radius:10px;" href="/index.html#settings/cellular">SET</a></li> </ul> </div> <div class="f"> <ul> <li>RSRP:<span id="rsrp"></span>dBm</li> <li>RSRQ:<span id="rsrq"></span>dB</li> <li>RSSI:<span id="rssi"></span>dBm</li> <li>SINR:<span id="sinr"></span>dB</li> <li>RQUALITY:<span id="radioQuality"></span></li> </ul> </div> <div class="f e5"> <ul> <li>5G RSRP:<span id="nr5rsrp"></span>dBm</li> <li>5G RSRQ:<span id="nr5rsrq"></span>dB</li> <li>SINR:<span id="nr5sinr"></span>dB</li> </ul> </div> <div class="f"> <ul> <li>4G MAIN:<span class="val" id="curBand"></span>(+<span class="val" id="SCCcount"></span>CA)</li> <li>ENB ID:<a id="lteitaly" target="lteitaly" href="#"><span id="enbid">#</span></a></li> <li>CELL ID:<span id="cellId">#</span></li> <li id="connectionText">Che la banda sia con te! Miononno &#9829;</li> </ul> </div> <div class="f"> <ul> <li>TOTAL:<span class="val" id="total">0</span></li> <li>SESSION:<span class="val" id="sessDuration"></span>min</li> </ul> </div> <div class="f"> <ul> <li>BATTERY:<span class="val" id="battChargeLevel"></span></li> <li>CHARGE:<span class="val" id="battChargeSource"></span></li> </ul> </div> <div class="f"> <ul> <li>WAN IP:<span class="val" id="IP"></span></li> </ul> </div> <div class="f"> <ul> <li>Temp</li> <li>device:<span class="val" id="devTemp"></span>Â°C</li> <li>battery:<span class="val" id="battTemp"></span>Â°C</li> </ul> </div> <div class="f field4g"> <ul> <li>4G BANDS:<span class="val" id="ltebands"></span></li> </ul> </div> <div class="f e5"> <ul> 5G BANDS:<span class="val" id="nr5gbands"></span> </ul> </div> </div>')
}
miawwan = null, version = "3.2", console.log("Code by Miononno - v" + version), console.log("type: resp"), window.setInterval(gInfo, 2500);
```
