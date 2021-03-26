# Request-for-scriptable

let fb = FileManager.iCloud();
let jsonConfig = fb.readString(fb.documentsDirectory()+"/TiempoEstimado/Config.txt")

let jsonConfigmax = fb.readString(fb.documentsDirectory()+"/TiempoEstimado/Configmax.txt")

let jsonConfig80 = fb.readString(fb.documentsDirectory()+"/TiempoEstimado/Config80.txt")

let jsonConfig80max = fb.readString(fb.documentsDirectory()+"/TiempoEstimado/Configmax80.txt")

let lineal = JSON.parse(jsonConfig)
let linealmax = JSON.parse(jsonConfigmax)
let lineal80 = JSON.parse(jsonConfig80)
let lineal80max = JSON.parse(jsonConfig80max)

let batteryLevel = Device.batteryLevel() * 100

let t80 = (lineal.b * 80 + lineal.a) - (lineal.b * batteryLevel + lineal.a)

let t80max = (linealmax.b * 80 + linealmax.a) - (linealmax.b * batteryLevel + linealmax.a)

let t100upper80 = (lineal80.b * 100 + lineal80.a) - (lineal80.b * batteryLevel + lineal80.a)

let t100maxupper80 = (lineal80max.b * 100 + lineal80max.a) - (lineal80max.b * batteryLevel + lineal80max.a)

let t100lower80 = (lineal80.b * 100 + lineal80.a) - (lineal80.b * 80 + lineal80.a)

let t100maxlower80 = (lineal80max.b * 100 + lineal80max.a) - (lineal80max.b * 80 + lineal80max.a)

let fecha = new Date();

(fecha.getHours() +':'+ fecha.getMinutes()); // Hora actual


if (batteryLevel > 80) {
  
 fecha.setMinutes(fecha.getMinutes() + t100upper80); 

  let hora100 = console.log(fecha.getHours() +':'+ fecha.getMinutes());

fecha.setMinutes(fecha.getMinutes() + t100maxupper80 - t100upper80); 

  let hora100max = console.log(fecha.getHours() +':'+ fecha.getMinutes());

}

else if (batteryLevel < 80) {
 fecha.setMinutes(fecha.getMinutes() + t80); 

  let hora80 = console.log(fecha.getHours() +':'+ fecha.getMinutes());

fecha.setMinutes(fecha.getMinutes() + t80max - t80); 

let hora80max = console.log(fecha.getHours() +':'+ fecha.getMinutes());

fecha.setMinutes(fecha.getMinutes() + t100lower80 - t80max + t80); 

  let hora100 = console.log(fecha.getHours() +':'+ fecha.getMinutes());

fecha.setMinutes(fecha.getMinutes() + t100maxlower80 - t100lower80 - t80 + t80max); 

  let hora100max = console.log(fecha.getHours() +':'+ fecha.getMinutes());

}




Script.complete()
