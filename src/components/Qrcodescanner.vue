<template>
  <div class="w-1/3 h-80 m-auto">
    <qrcode-stream @decode="decode" :track="drawOutline"></qrcode-stream>
  </div>
</template>
<script>
import { QrcodeStream } from "vue3-qrcode-reader";
export default {
  components: {
    QrcodeStream,
  },
  methods: {
    decode(url) {
      window.location.href = url; //okutulan qr code'un url adresini al ve oku
    },
    drawOutline(decodeData, context) {
      var points = [];
      for (var k in decodeData) {
        switch (k) {
          case "topLeftCorner":
            points[0] = decodeData[k];
            break;
          case "topRightCorner":
            points[1] = decodeData[k];
            break;
          case "bottomRightCorner":
            points[2] = decodeData[k];
            break;
          case "bottomLeftCorner":
            points[3] = decodeData[k];
            break;
          default:
            break;
        }
      }
      context.lineWidth = 10;
      context.strokeStyle = "green";
      context.beginPath();

      context.moveTo(points[0].x, points[0].y);
      for (const { x, y } of points) {
        context.lineTo(x, y);
      }
      context.lineTo(points[0].x, points[0].y);
      context.closePath();
      context.stroke();
    },
  },
};
</script>