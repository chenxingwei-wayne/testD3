# testD3

import { Component, OnInit, ViewChild, ElementRef } from '@angular/core';
import * as d3 from "d3";
@Component({
  selector: 'app-canvas',
  templateUrl: './canvas.component.html',
  styleUrls: ['./canvas.component.css']
})
export class CanvasComponent implements OnInit {
  @ViewChild('myCanvas', { static: false }) myCanvas: ElementRef;
  public context: CanvasRenderingContext2D;
  constructor() { }

  ngOnInit() {

  }
  walkX = d3.scaleLinear()
    .domain([0, 300])
    .range([25, 300])
  walkY = d3.scaleLinear()
    .domain([0, 200])
    .range([140, 0])

  lineGenerator: any = d3.line().x((d: any) => this.walkX(d.step))
    .y((d: any) => this.walkY(d.value)).curve(d3.curveCardinal);
  points: any = [
    { step: 0, value: 80 },
    { step: 10, value: 100 },
    { step: 20, value: 30 },
    { step: 30, value: 30 },
    { step: 40, value: 60 },
    { step: 50, value: 70 },
    { step: 60, value: 70 },
    { step: 70, value: 10 },
    { step: 80, value: 0 },
    { step: 90, value: 0 },
    { step: 100, value: 0 },
    { step: 110, value: 0 },
    { step: 120, value: 0 },
    { step: 130, value: 0 },
    { step: 140, value: 30 },
    { step: 150, value: 30 },
    { step: 160, value: 30 },
    { step: 170, value: 30 },
    { step: 180, value: 30 },
    { step: 190, value: 30 },
    { step: 200, value: 30 },
    { step: 210, value: 30 },
    { step: 220, value: 30 },
    { step: 230, value: 30 },
    { step: 240, value: 30 },
    { step: 250, value: 30 },
    { step: 275, value: 30 },
  ];
  width: any;
  ngAfterViewInit(): void {
    this.context = (<HTMLCanvasElement>this.myCanvas.nativeElement).getContext('2d');
    this.lineGenerator.context(this.context);
    this.context.strokeStyle = '#999';
    this.context.beginPath();
    this.lineGenerator(this.points);
    this.context.stroke();
    this.width = this.myCanvas.nativeElement.width;
    // 启用定时器
    this.refreshArr();
  }
  intervalId = 0;
  private clearTimer() {
    console.log('stop refreshing');
    clearInterval(this.intervalId);
  }
  // 生成指定范围内的随机数
  private getRandom(begin, end) {
    return Math.floor(Math.random() * (end - begin));
  }
  // 元素关闭清除定时器
  ngOnDestroy() { this.clearTimer(); }
  // 启动定时刷新数组
  refreshArr() {
    this.clearTimer()
    this.intervalId = window.setInterval(() => {
      this.points = [];
      for (let i = 0; i < 30; i++) {
        this.points.push({
          step: i * 10, value: this.getRandom(0, 100)
        });
      }
      // 重新设置宽度用于清空canvas
      this.myCanvas.nativeElement.width = this.width;
      // 利用最新的数组重新绘制画布
      this.context = (<HTMLCanvasElement>this.myCanvas.nativeElement).getContext('2d');
      this.lineGenerator.context(this.context);
      this.context.strokeStyle = '#999';
      this.context.beginPath();
      this.lineGenerator(this.points);
      this.context.stroke();
    }, 3000);
  }
  // 停止定时刷新数组
  stopRefresh() {
    this.clearTimer();
  }


}


<p>canvas works!</p>
<canvas #myCanvas style="width: 300px; height: 200px;"></canvas>
