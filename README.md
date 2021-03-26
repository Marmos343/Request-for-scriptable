//Request-for-scriptable

//first lines extract 4 JSONS with numbers that help to calculate the time left to 80% and 100%



let fb = FileManager.iCloud();
let jsonConfig = fb.readString(fb.documentsDirectory()+"/TiempoEstimado/Config.txt")
//this action extract this {"b":0.7988,"a":-22.9268}
//this data is different for each device

let jsonConfigmax = fb.readString(fb.documentsDirectory()+"/TiempoEstimado/Configmax.txt")
//this action extract this {"b":1.2174,"a":-6.0405}
//this data is different for each device

let jsonConfig80 = fb.readString(fb.documentsDirectory()+"/TiempoEstimado/Config80.txt")
//this action extract this {"b":3.2000,"a":-256.0000}
//this data is different for each device

let jsonConfig80max = fb.readString(fb.documentsDirectory()+"/TiempoEstimado/Configmax80.txt")
// this action extract this {"b":4.1500,"a":-212.0000}
//this data is different for each device

let lineal = JSON.parse(jsonConfig)
let linealmax = JSON.parse(jsonConfigmax)
let lineal80 = JSON.parse(jsonConfig80)
let lineal80max = JSON.parse(jsonConfig80max)

let batteryLevel = Device.batteryLevel() * 100

let t80 = (lineal.b * 80 + lineal.a) - (lineal.b * batteryLevel + lineal.a)
// here calculate time left to 80% in minutes

let t80max = (linealmax.b * 80 + linealmax.a) - (linealmax.b * batteryLevel + linealmax.a)
// here calculate the maximum time left to 80%

let t100upper80 = (lineal80.b * 100 + lineal80.a) - (lineal80.b * batteryLevel + lineal80.a)
// here calculate time left to 100% when the battery is upper 80%

let t100maxupper80 = (lineal80max.b * 100 + lineal80max.a) - (lineal80max.b * batteryLevel + lineal80max.a)
// here calculate max time left to 100% when the battery is upper 80%

let t100lower80 = (lineal80.b * 100 + lineal80.a) - (lineal80.b * 80 + lineal80.a)
// here calculate time left to 100% when battery is lower than 80% but it must add to time left to 80%

let t100maxlower80 = (lineal80max.b * 100 + lineal80max.a) - (lineal80max.b * 80 + lineal80max.a)
// here calculate time left to 100% when batttery lower than 80% but it must add to maximum time left to 80%

let fecha = new Date();

(fecha.getHours() +':'+ fecha.getMinutes());


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
