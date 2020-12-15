# Sample Data

List of sample products

```JSON
POST /ecomm-products/_doc/P0001
{
 "product_id" : "P0001",  
 "name" : {
  "it" : "Apple iPhone 12",
  "en" : "Apple iPhone 12"
 },
 "short_description" : {
  "it" : "iPhone 12 Pro e iPhone 12 Pro Max sono resistenti agli schizzi, all'acqua e alla polvere e sono stati testati in condizioni di laboratorio controllate con una classificazione IP68 secondo lo standard IEC 60529 (profondità massima di 6 metri fino a 30 minuti).",
  "en" : "iPhone 12 Pro and iPhone 12 Pro Max are splash, water, and dust resistant and were tested under controlled laboratory conditions with a rating of IP68 under IEC standard 60529 (maximum depth of 6 meters up to 30 minutes)."
 },
 "brand" : {
  "id" : "B0001",
  "name" : "Apple"
 },
 "categories" : [
  {
   "id" : "C0001",
   "description" : {
    "it" : "Telefonia & Cellulari",
    "en" : "Telephony and Mobile phones"
   }
  },
  {
   "id" : "C0002",
   "description" : {
    "it" : "Smartphone",
    "en" : "Smartphones"
   }
  }
 ],
 "prices" : {
  "site_IT" :
  {
   "value": 879.90,
   "currency": "EUR"
  },
  "site_EU" :
  {
   "value": 899.90,
   "currency": "EUR"
  },
  "site_UK" : { 
   "value": 799.90,
   "currency": "GPB"
  }
 }
}
```

```JSON
POST /ecomm-products/_doc/P0002
{
 "product_id": "P0002",
 "name" : {
  "it" : "Apple iPhone 11",
  "en" : "Apple iPhone 11"
 },
 "short_description" : {
  "it" : "iPhone 11 è uno smartphone progettato e prodotto dalla Apple. Le caratteristiche principali sono il nuovo chip Apple A13 Bionic, e il sistema delle doppie fotocamere da 12 MP ultra-wide.",
  "en" : "iPhone 11 is a smartphone designed and manufactured by Apple. The main features are the new Apple A13 Bionic chip, and the ultra-wide 12 MP dual camera system."
 },
 "brand" : {
  "id" : "B0001",
  "name" : "Apple"
 },
 "categories" : [
  {
   "id" : "C0001",
   "description" : {
    "it" : "Telefonia & Cellulari",
    "en" : "Telephony and Mobile phones"
   }
  },
  {
   "id" : "C0002",
   "description" : {
    "it" : "Smartphone",
    "en" : "Smartphones"
   }
  }
 ],
 "prices" : {
  "site_IT" : {
   "value": 679.90,
   "currency": "EUR"
  },
  "site_EU" : {
   "value": 699.90,
   "currency": "EUR"
  },
  "site_UK" : { 
   "value": 599.90,
   "currency": "GPB"
  } 
 }
}
```

```JSON
POST /ecomm-products/_doc/P0003
{
 "product_id": "P0003",
 "name" : {
  "it" : "OnePlus Nord",
  "en" : "OnePlus Nord"
 },
 "short_description" : {
  "en": "An authentic 48MP war machine that we took directly from the OnePlus 8. It captures all the details even in low light conditions. Speed up and optimized software with hundreds of optimizations. Fluid. Quick. Reactive. Oddly hypnotic. 90Hz encompasses all of this.",
  "it": "Un’autentica macchina da guerra a 48MP che abbiamo preso direttamente dal OnePlus 8. Cattura tutti i dettagli anche in condizioni di scarsa luminosità. Software velocizzato e ottimizzato con centinaia di ottimizzazioni. Fluido. Rapido. Reattivo. Stranamente ipnotico. I 90Hz racchiudono tutto questo."
 },
 "brand" : {
  "id" : "B0010",
  "name": "OnePlus"
 },
 "categories" : [
  {
   "id" : "C0001",
   "description" : {
    "it" : "Telefonia & Cellulari",
    "en" : "Telephony and Mobile phones"
   }
  },
  {
   "id" : "C0002",
   "description" : {
    "it" : "Smartphone",
    "en" : "Smartphones"
   }
  }
 ],
 "prices" : {
  "site_IT" : {
   "value": 479.90,
   "currency": "EUR"
  },
  "site_EU" : {
   "value": 499.90,
   "currency": "EUR"
  },
  "site_UK" : { 
   "value": 399.90,
   "currency": "GPB"
  }
 }
}
```

```JSON
POST /ecomm-products/_doc/P0004
{
 "product_id": "P0004",
 "name" : {
  "it" : "OnePlus 8T",
  "en" : "OnePlus 8T"
 },
 "short_description" : {
  "en": "The OnePlus 8T is powered by the Snapdragon 865 processor with the Adreno 650 GPU, accompanied by 128 or 256 GB of non-expandable UFS 3.1 storage and 8 or 12 GB of LPDDR4X RAM. It has stereo speakers with active noise cancellation, but no audio jack. Biometric options include an optical (in-display) fingerprint scanner and facial recognition. The display is a 6.55-inch 1080p AMOLED with a 20:9 aspect ratio and HDR10+ support, identical to the 8 aside from the refresh rate, which has been increased from 90 Hz to 120 Hz. The battery capacity is slightly larger than the 8 at 4500 mAh. Fast-charging is supported at up to 65 W via Warp Charge 65 compared to the 8's Warp Charge 30. This is enabled by a dual-cell design, with the battery split into two 2250 mAh cells.[4] Like the 8, wireless charging is not supported.",
  "it": "OnePlus 8T è alimentato dal processore Snapdragon 865 con GPU Adreno 650, accompagnato da 128 o 256 GB di memoria UFS 3.1 non espandibile e 8 o 12 GB di RAM LPDDR4X. Ha altoparlanti stereo con cancellazione attiva del rumore, ma nessun jack audio. Le opzioni biometriche includono uno scanner di impronte digitali ottico (in-display) e il riconoscimento facciale. Il display è un AMOLED da 6,55 pollici da 1080p con proporzioni 20:9 e supporto HDR10+, identico all'8 a parte la frequenza di aggiornamento, che è stata aumentata da 90 Hz a 120 Hz. La capacità della batteria è leggermente superiore all'8 a 4500 mAh. La ricarica rapida è supportata fino a 65 W tramite Warp Charge 65 rispetto alla carica di curvatura 30 dell'8. Questo è abilitato da un design a doppia cella, con la batteria divisa in due celle da 2250 mAh. [4] Come l'8, la ricarica wireless non è supportata."
 },
 "brand" : {
  "id" : "B0010",
  "name": "OnePlus"
 },
 "categories" : [
  {
   "id" : "C0001",
   "description" : {
    "it" : "Telefonia & Cellulari",
    "en" : "Telephony and Mobile phones"
   }
  },
  {
   "id" : "C0002",
   "description" : {
    "it" : "Smartphone",
    "en" : "Smartphones"
   }
  }
 ],
 "prices" : {
  "site_EU" : {
   "value": 699.90,
   "currency": "EUR"
  },
  "site_UK" : { 
   "value": 599.90,
   "currency": "GPB"
  }
 }
}
```

[&lt; Back to summary](Ecomm_Sample.md)