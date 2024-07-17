// Decode uplink function.
function decodeUplink(input) {
  const payload = String.fromCharCode.apply(null, input.bytes);
  const data = {};

  payload.split(';').forEach(pair => {
    const [key, value] = pair.split(':');
    if (key === 't') {
      data.temp = parseFloat(value);
    } else if (key === 'h') {
      data.humidity = parseFloat(value);
    } else if (key === 'RO1') {
      data.RO1 = parseInt(value);
    }
  });

  return {
    data: data
  };
}

function encodeDownlink(input) {

  var i = 0;
  var output = [];

  input.data.array.forEach(( item ) => {

    switch ( item.name ) {
      case "digital_out":
        item.type = 1;
        break;
      default:
        item.type = 255;
        break;
    }

    output[i++] = item.channel;
    output[i++] = item.type;
    output[i++] = item.value;

  });

  return {
    bytes: output
  };
}

// test the ChirpStack decoder using quickjs interpreter
// 
//const input = { "data": {"array":[{"channel":4,"name":"digital_out","value":1}]} };
//var result = encodeDownlink(input);
//print (JSON.stringify( result ));