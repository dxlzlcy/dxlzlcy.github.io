## working with data

[link](https://www.youtube.com/watch?v=tc8DU14qX6I&list=PLRqwX-V7Uu6YxDKpFzf_2D84p0cyk4T7X&index=3)

[本地文件夹路径](/Users/liuchengyi/刘承翊/vscode-files/js_files/code_train_youtube)

### fetch

```html
<body>
    <img src="" alt="" id="rb" width="480">
    <script>
        // console.log('hello');
        fetch('rainbow.jpg').then(res => {
            return res.blob();
        }).then(blob => {
            document.getElementById('rb').src = URL.createObjectURL(blob);
        })
    </script>
    <div>hello world</div>
    <div>你好</div>

</body>
```

改成async函数

```html
<img src="" alt="" id="rb" width="480">
<script>
  catch_rainbow()
    .then(response => {
    console.log('good');
  })
    .catch(error => {
    console.log('error');
  });

  async function catch_rainbow() {
    const res = await fetch('rainbow.jpg');
    const blob = await res.blob();
    document.getElementById('rb').src = URL.createObjectURL(blob);
  }
</script>
```

### 读取csv数据

```html
<script>
  get_csv_data();

  async function get_csv_data() {
    const res = await fetch('ZonAnn.Ts+dSST.csv');
    const data = await res.text();
    // console.log(data);
    const rows = data.split('\n').slice(1);
    // console.log(rows.slice(1, ));
    rows.forEach(row => {
      if (!row) return;
      year = row.split(',')[0];
      temp = row.split(',')[1];
      console.log(year, temp);
    })

  }
</script>
```

### 用chart.js展示

```html
<canvas id='chart' width='400' height='200'></canvas>
<script src="https://cdnjs.cloudflare.com/ajax/libs/Chart.js/2.8.0/Chart.bundle.min.js"></script>
<script>
  // const chart = require('chart');

  async function get_csv_data() {
    x = [];
    y = [];
    const res = await fetch('ZonAnn.Ts+dSST.csv');
    const data = await res.text();
    // console.log(data);
    const rows = data.split('\n').slice(1);
    // console.log(rows.slice(1, ));
    rows.forEach(row => {
      if (!row) return;
      year = row.split(',')[0];
      temp = row.split(',')[1];
      x.push(year);
      y.push(parseFloat(temp) + 14);
      console.log(year, temp);
    })
    return {
      x,
      y
    }
  }

  async function chart_it() {
    const data = await get_csv_data();
    const ctx = document.getElementById('chart').getContext('2d');
    const mychart = new Chart(ctx, {
      type: 'line',
      // type: 'bar',
      data: {
        labels: data.x,
        datasets: [{
          fill: false,
          data: data.y,
          backgroundColor: 'rgba(255,100,130,0.2)',
          borderColor: 'rgba(255,100,130,1)',
          borderWidth: 1,
          label: 'global temperature'
        }]
      },
      options: {
        scales: {
          yAxes: [{
            ticks: {
              callback: function (value, index, values) {
                return value + '°';
              }
            }
          }]
        }
      }
    })

    }

  chart_it();
</script>
```

[chart.js教程地址](https://www.chartjs.org/docs/latest/configuration/legend.html)

