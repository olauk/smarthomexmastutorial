# Selvvannende julegran

## Selvvannende julegran @showdialog

Jula nærmer seg og dere skal lage en løsning som sikrer at julegrana holder seg grønn og frisk gjennom jula.


    Mål: Når det er for lite vann, skal micro: bit fylle opp beholderen med vann.

## Sett opp OLED

Vi starter med oppkobling av OLED - skjermen.
    Finn`` || oled: initialize OLED with width 128 height 64 || `` og plasser denne i`` || basic: ved start || ``
Ikke endre verdiene i denne blokken.

```blocks
OLED.init(128, 64)
```

## Koble til fuktighetssensor @showdialog

Vi kobler fuktighetssensor til P1.Pass på at sort ledning kobles mot sort pin på "Sensor:bit" - brettet.

## Lag variabel for fuktighetssensor

For å huske verdien fra fuktighetssensoren lager vi en variabel.Velg "Lag en variabel" fra`` || variables: variabler || ``.Denne kan hete "fuktighet".
    Sett "fuktighet" til 0 i ved start.

```blocks
OLED.init(128, 64)
let fuktighet = 0

```

## Les av verdi fra fuktighetssensor

Vi vil lese av verdien som fuktighetssensoren måler når den er i luft og når den er i vann.Vi henter blokken`` || loops: Every 500 ms || `` fra løkker.
Vi vil oppdatere variabelen "fuktighet" hvert 500 milisekund, så finn`` || variables: sett fuktighet til || `` og legg den i "every 500 ms".
Fra kategorien "Smarthome" finner vi`` || smarthome: value of soil moisture at pin P1 || ``.Legg denne blokken slik at variabelen fuktighet settes til "value of soil moisture.."

```blocks
let fuktighet = 0
loops.everyInterval(500, function () {
    fuktighet = smarthome.ReadSoilHumidity(AnalogPin.P1)
})
```

## Skriv verdi fra fuktighetssensor på OLED - displayet

Det er lettere å lese hvis vi skriver disse verdiene på OLED - displayet.Plasser disse i`` || basic: gjenta for alltid: || ``

Finn`` || oled: show(without newline) string || `` og skriv "fuktighet: ".
Finn så`` || oled: show(without newline) number || `` og sett inn variabelen "fuktighet".For at vi ikke skal skrive en ny linje hvert 500 ms, så legger vi inn en`` || basic: pause || `` på 200ms og bruker`` || oled: clear OLED display || ``. 


```blocks
basic.forever(function () {
    OLED.writeString("Fuktighet:")
    OLED.writeNum(fuktighet)
    basic.pause(200)
    OLED.clear()
})
```

## Noter verdier @showdialog

Noter ned verdien som fuktighetssensoren viser når den er tørr og verdien når dere har fuktighetssensoren i vann.
Dette skal dere bruke for å automatisere juletrevanneren.

## Koble opp rele og pumpe @showdialog

Vi skal nå koble opp pumpen.

Der må vi bruke et rele for å skru strømmen av og på.

Bruk en ledning fra P2 til releet.

Koble den røde ledninge fra pumpen til den midterste skrukontakten på releet.

Koble en enkel ledning fra skrukoblingen NO på releet til rød på sensor: bit.

Koble den svarte ledningen til svart på sensor: bit
![Oppkobling](/static/tutorials / LRBAV68.png)

## Test av pumpen

Vi programmerer en liten test for å se om vi har koblet riktig.Finn`` || input: når knapp A trykkes || `` og plasser`` || smarthome: Relay P2 toggle to NC Open NO Close || ``
Gjør det samme med`` || input: når knapp B trykkes || `` men sett`` || smarthome: Relay P2 toggle to NC Close NO Open || `` til "NC Close NO Open"

    ```blocks
input.onButtonPressed(Button.A, function () {
    smarthome.Relay(DigitalPin.P2, smarthome.RelayStateList.Off)
})
input.onButtonPressed(Button.B, function () {
    smarthome.Relay(DigitalPin.P2, smarthome.RelayStateList.On)
})
```
## Automatisere vanning

Vi vil at programmet skal styre vanningen.

    Finn`` || logic: hvis sann så ellers || `` og plasser den i`` || basic: gjenta for alltid || ``

Finn`` || logic: 0 < 0 || `` og plasser den der det står "sann" i "hvis sann så ellers" - blokken.

```blocks
basic.forever(function () {
    OLED.writeString("Fuktighet:")
    OLED.writeNum(fuktighet)
    if (0 < 0) {
    	
    } else {
    	
    }
    basic.pause(200)
    OLED.clear()
})
``` 

## Automatiser vanning steg 2

Bruk verdiene dere noterte fra testen med fuktighetssensoren.Hva viser sensoren når den er helt tørr ? Hva viser sensoren når den såvidt er nede i vannet ?
    Viser sensoren forskjellig verdi ut fra hvor langt dere fører den ned i vannet ?

        Hvis fuktighetssensoren er tørr, så går vi ut fra at julegrana må vannes.Hvis fuktighetssensoren oppdager vann, så antar vi at vi har vannet nok.
            Finn`` || variables: fuktighet || `` og plasser den i`` || logic: hvis sann så ellers || `` og skriv inn verdien som dere målte når fuktighetssensoren var i luft.

```blocks
basic.forever(function () {
    OLED.writeString("Fuktighet:")
    OLED.writeNum(fuktighet)
    if (fuktighet < 10) {
    	
    } else {
    	
    }
    basic.pause(200)
    OLED.clear()
})
```

## Automatiser vanning steg 3

Hvis fuktighetssensor viser verdien som tilsvarer at den er tørr - så skal pumpen skrus på.
Hvis fuktighetssensor viser verdien som tilsvarer at den er i vann - så skal pumpen skrus av.

    Bruk`` || smarthome: Relay P2 toggle to NC Open NO Close || `` for å skru på pumpen.Bruk samme blokk, men endre til "NC|Close NO|Open" for å skru pumpen av.

```blocks
basic.forever(function () {
    OLED.writeString("Fuktighet:")
    OLED.writeNum(fuktighet)
    if (fuktighet < 10) {
        smarthome.Relay(DigitalPin.P2, smarthome.RelayStateList.Off)
    } else {
        smarthome.Relay(DigitalPin.P2, smarthome.RelayStateList.On)
    }
    basic.pause(200)
    OLED.clear()
})
```

## Forbedre programmet!

Forslag til problemer som kan forbedres:
- Hvordan hindre at man fyller for mye vann ?
    - Hva skjer hvis koppen som pumpen fyller fra er tom ?
        - Kan man se på skjermen om julegrana har vann eller ikke ?

            Lykke til!

                ```package
smarthome=github:tinkertanker/pxt-smarthome
```

                ```template
loops.everyInterval(500)
smarthome.ReadSoilHumidity(AnalogPin.P1)

```
