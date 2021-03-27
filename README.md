//first lines extract 4 JSONS with numbers that help to calculate the time left to 80% and 100%

let fb = FileManager.iCloud();
let jsonConfig = fb.readString(fb.documentsDirectory()+"/TiempoEstimado/Config.txt");
//this action extract this {"b":0.7988,"a":-22.9268}
//this data is different for each device

let jsonConfigmax = fb.readString(fb.documentsDirectory()+"/TiempoEstimado/Configmax.txt");
//this action extract this {"b":1.2174,"a":-6.0405}

let jsonConfig80 = fb.readString(fb.documentsDirectory()+"/TiempoEstimado/Config80.txt");
//this action extract this {"b":3.2000,"a":-256.0000}

let jsonConfig80max = fb.readString(fb.documentsDirectory()+"/TiempoEstimado/Configmax80.txt");
// this action extract this {"b":4.1500,"a":-212.0000}

let lineal = JSON.parse(jsonConfig);
let linealmax = JSON.parse(jsonConfigmax);
let lineal80 = JSON.parse(jsonConfig80);
let lineal80max = JSON.parse(jsonConfig80max);

let batteryLevel = Device.batteryLevel() * 100;

let t80 = (lineal.b * 80 + lineal.a) - (lineal.b * batteryLevel + lineal.a);
//here calculates the time left to 80% this result is in minutes

let t80max = (linealmax.b * 80 + linealmax.a) - (linealmax.b * batteryLevel + linealmax.a);
//here calculates the maximum time left to 80% this result is in minutes

let t100upper80 = (lineal80.b * 100 + lineal80.a) - (lineal80.b * batteryLevel + lineal80.a);
// here calculates the time left to 100% when the battery is upper than 80%

let t100maxupper80 = (lineal80max.b * 100 + lineal80max.a) - (lineal80max.b * batteryLevel + lineal80max.a);
// here calculates the maximum time left to 100% when the battery is upper than 80%

let t100lower80 = (lineal80.b * 100 + lineal80.a) - (lineal80.b * 80 + lineal80.a);
// here calculares the total time to 100% from 80%. for example: if batery is 26% and you want to know the time left to 100% --> t80 + t100lower80 = minutes left to 100%

let t100maxlower80 = (lineal80max.b * 100 + lineal80max.a) - (lineal80max.b * 80 + lineal80max.a);
// here calculates the maximum time left to 100% from 80%

let fecha = new Date();
// use it for time left to 80%

let fechamax = new Date();
// use it for maximum time left to 80%

let fecha80 = new Date();
// use it for time left to 100%

let fecha80max = new Date();
// use it for maximum time left to 100%

if (batteryLevel > 80) {

fecha80.setMinutes(fecha80.getMinutes() + t100upper80); 

fecha80max.setMinutes(fecha80max.getMinutes() + t100maxupper80);

let horasLineal80 = (fecha80.getHours().toString().length > 1) ? fecha80.getHours().toString() : "0" + fecha80.getHours().toString();
  
  let minutosLineal80 = (fecha80.getMinutes().toString().length > 1) ? fecha80.getMinutes().toString() : "0" + fecha80.getMinutes().toString();
    
  let horasLineal80max = (fecha80max.getHours().toString().length > 1) ? fecha80max.getHours().toString() : "0" + fecha80max.getHours().toString();
  
  let minutosLineal80max = (fecha80max.getMinutes().toString().length > 1) ? fecha80max.getMinutes().toString() : "0" + fecha80max.getMinutes().toString();

let w = new ListWidget()

w.addText("Batería actual " + Math.round(batteryLevel) + "%").font =
Font.semiboldRoundedSystemFont(24);
w.backgroundColor = Color.clear();

let s = w.addStack();

agregarTexto(s, "Coming", "resumens", "link");

w.presentMedium();

function agregarTexto(stack, titulo, resumen, link){
  let s2 = stack.addStack()
  s2.layoutVertically();

let t = s2.addText("• Carga al 100% a las " + horasLineal80 + ":" + minutosLineal80 + " Máx. a las " + horasLineal80max + ":" + minutosLineal80max).font = Font.semiboldRoundedSystemFont(16);

  }
Script.complete()
}

else if (batteryLevel < 80) {

fecha.setMinutes(fecha.getMinutes() + t80);
  
fechamax.setMinutes(fechamax.getMinutes() + t80max); 

fecha80.setMinutes(fecha80.getMinutes() + t80 + t100lower80); 

fecha80max.setMinutes(fecha80max.getMinutes() + t80max + t100maxlower80); 


// Creating widget when batteryLevel is lower 80%

let w = new ListWidget()

w.addText("Batería actual " + Math.round(batteryLevel) + "%").font =
Font.semiboldRoundedSystemFont(24);
w.backgroundColor = Color.clear()

let s = w.addStack();

agregarTexto(s, "Coming", "resumens", "link");

w.presentMedium();

function agregarTexto(stack, titulo, resumen, link){
  let s2 = stack.addStack();
  s2.layoutVertically();
  
  let horasLineal = (fecha.getHours().toString().length > 1) ? fecha.getHours().toString() : "0" + fecha.getHours().toString();
  
  let minutosLineal = (fecha.getMinutes().toString().length > 1) ? fecha.getMinutes().toString() : "0" + fecha.getMinutes().toString();
  
  let horasLinealMax = (fechamax.getHours().toString().length > 1) ? fechamax.getHours().toString() : "0" + fechamax.getHours().toString();
  
  let minutosLinealMax = (fechamax.getMinutes().toString().length > 1) ? fechamax.getMinutes().toString() : "0" + fechamax.getMinutes().toString();
  
  let horasLineal80 = (fecha80.getHours().toString().length > 1) ? fecha80.getHours().toString() : "0" + fecha80.getHours().toString();
  
  let minutosLineal80 = (fecha80.getMinutes().toString().length > 1) ? fecha80.getMinutes().toString() : "0" + fecha80.getMinutes().toString();
    
  let horasLineal80max = (fecha80max.getHours().toString().length > 1) ? fecha80max.getHours().toString() : "0" + fecha80max.getHours().toString();
  
  let minutosLineal80max = (fecha80max.getMinutes().toString().length > 1) ? fecha80max.getMinutes().toString() : "0" + fecha80max.getMinutes().toString();
  
  s2.addText("• Carga al 80% a las " + horasLineal + ":" + minutosLineal + " Máx. a las " + horasLinealMax + ":" + minutosLinealMax).font = Font.semiboldRoundedSystemFont(16);

let t = s2.addText("• Carga al 100% a las " + horasLineal80 + ":" + minutosLineal80 + " Máx. a las " + horasLineal80max + ":" + minutosLineal80max).font = Font.semiboldRoundedSystemFont(16);

  }

}

Script.complete()
