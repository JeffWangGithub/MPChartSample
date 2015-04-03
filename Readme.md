
###说明
本实例是MPChart如何使用的一个Demo。
####MPChart
MPChart是一个第三方的绘图工具库，支持柱形图，折线图，平行图，网状图等。
其优点在于：
- 使用简单，界面很清新
- 与Achart相比，对动画的支持很好。
![](http://www.glanwang.com/android/_image/mpchart%E7%BB%98%E5%9B%BE%E5%BA%93%E7%9A%84%E4%BD%BF%E7%94%A8/MPChartView.gif)


###具体使用
1 xml或者代码声明View

```
<ScrollView
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    xmlns:android="http://schemas.android.com/apk/res/android">
    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:orientation="vertical">
        <!--折线图-->
        <com.github.mikephil.charting.charts.LineChart
            android:id="@+id/line_chart"
            android:layout_width="match_parent"
            android:layout_height="200dp" />
        <!--柱形图-->
        <com.github.mikephil.charting.charts.BarChart
            android:id="@+id/bar_chart"
            android:layout_width="match_parent"
            android:layout_height="200dp" />
        <!--饼形图-->
        <com.github.mikephil.charting.charts.PieChart
            android:id="@+id/pie_chart"
            android:layout_width="match_parent"
            android:layout_height="345dp" />
    </LinearLayout>
</ScrollView>
```
2 设置样式，并填充数据
```


public class MainActivity extends ActionBarActivity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        //折线图
        LineChart mLineChart = (LineChart) findViewById(R.id.line_chart);
        setLineChart(mLineChart);
        loadLineChartData(mLineChart);
        //柱形图
        BarChart mBarChart = (BarChart) findViewById(R.id.bar_chart);
        setBarChart(mBarChart);
        loadBarChartData(mBarChart);
        //饼形图
        PieChart mPieChart = (PieChart) findViewById(R.id.pie_chart);
        setPieChart(mPieChart);
        loadPieChartData(mPieChart);
    }

    private void loadPieChartData(PieChart chart) {
        //设置中心数据
        chart.setCenterText("GlanWang");

        ArrayList<Entry> entries = new ArrayList<Entry>();

        for (int i = 0; i < 4; i++) {
            entries.add(new Entry((int) (Math.random() * 70) + 30, i));
        }

        PieDataSet mPieDataSet = new PieDataSet(entries, "");
        // space between slices
        mPieDataSet.setSliceSpace(2f);
        mPieDataSet.setColors(ColorTemplate.VORDIPLOM_COLORS);

        PieData mPieChartData = new PieData(getQuarters(),mPieDataSet);

        // set data
        chart.setData( mPieChartData);

        //设置动画
        chart.animateXY(900, 900);

        //设置图例
        Legend l = chart.getLegend();
        l.setPosition(Legend.LegendPosition.RIGHT_OF_CHART);

        mPieChartData.setValueFormatter(new PercentFormatter());//设置显示成百分比
//        mChartData.setValueTypeface(mTf);
        mPieChartData.setValueTextSize(11f);//设置文字大小
        mPieChartData.setValueTextColor(Color.WHITE);
    }

    /**
     * 设置饼形图样式
     * @param chart
     */
    private void setPieChart(PieChart chart) {
        // apply styling
        chart.setDescription("");
        chart.setHoleRadius(52f);
        chart.setTransparentCircleRadius(57f);
        chart.setCenterText("MPChart\nAndroid");
//        chart.setCenterTextTypeface(mTf);
        chart.setCenterTextSize(18f);
        chart.setUsePercentValues(true);

        //中心文字颜色
        chart.setCenterTextColor(Color.GREEN);
    }

    /**
     * 加载并设置柱形图的数据
     * @param chart
     */
    private void loadBarChartData(BarChart chart) {
        //所有数据点的集合
        ArrayList<BarEntry> entries = new ArrayList<>();
        for (int i = 0; i <12; i++) {
            entries.add(new BarEntry((int) (Math.random() * 70) + 30, i));
        }
        //柱形数据的集合
        BarDataSet mBarDataSet = new BarDataSet(entries,"barDataSet");
        mBarDataSet.setBarSpacePercent(20f);
        mBarDataSet.setColors(ColorTemplate.VORDIPLOM_COLORS);//设置每条柱子的颜色
        mBarDataSet.setHighLightAlpha(255);//设置点击后高亮颜色透明度
        mBarDataSet.setHighLightColor(Color.BLUE);

        //BarData表示挣个柱形图的数据
        BarData mBarData = new BarData(getXAxisShowLable(),mBarDataSet);
        chart.setData(mBarData);
        chart.animateY(1500);//设置动画
    }

    /**
     * 设置柱形图的样式
     * @param chart
     */
    private void setBarChart(BarChart chart) {
        chart.setDescription("Glan");
        chart.setDrawGridBackground(false);//设置网格背景
        chart.setScaleEnabled(false);//设置缩放
        chart.setDoubleTapToZoomEnabled(false);//设置双击不进行缩放

        //设置X轴
        XAxis xAxis = chart.getXAxis();
        xAxis.setPosition(XAxis.XAxisPosition.BOTTOM);//设置X轴的位置
//        xAxis.setTypeface(mTf);//设置字体
        xAxis.setDrawGridLines(false);
        xAxis.setDrawAxisLine(true);

        //获得左侧侧坐标轴
        YAxis leftAxis = chart.getAxisLeft();
//        leftAxis.setTypeface(mTf);
        leftAxis.setLabelCount(5);
//        leftAxis.setAxisLineWidth(1.5f);

        //设置右侧坐标轴
        YAxis rightAxis = chart.getAxisRight();
        rightAxis.setDrawAxisLine(false);//右侧坐标轴线
        rightAxis.setDrawLabels(false);//右侧坐标轴数组Lable
//        rightAxis.setTypeface(mTf);
//        rightAxis.setLabelCount(5);
//        rightAxis.setDrawGridLines(false);
    }

    /**
     * 为折线图设置数据
     * @param chart
     */
    private void loadLineChartData(LineChart chart){
        //所有线的List
        ArrayList<LineDataSet> allLinesList = new ArrayList<LineDataSet>();

        ArrayList<Entry> entryList = new ArrayList<Entry>();
        for(int i=0;i<12;i++){
            //Entry(yValue,xIndex);一个Entry表示一个点，第一个参数为y值，第二个为X轴List的角标
            entryList.add(new Entry((int)(Math.random()*65)+40,i));
        }
        //LineDataSet可以看做是一条线
        LineDataSet dataSet1 = new LineDataSet(entryList,"dataLine1");
        dataSet1.setLineWidth(2.5f);
        dataSet1.setCircleSize(4.5f);
        dataSet1.setHighLightColor(Color.RED);//设置点击某个点时，横竖两条线的颜色
        dataSet1.setDrawValues(false);//是否在点上绘制Value

        allLinesList.add(dataSet1);

        //LineData表示一个LineChart的所有数据(即一个LineChart中所有折线的数据)
        LineData mChartData = new LineData(getXAxisShowLable(),allLinesList);

        // set data
        chart.setData((LineData) mChartData);
        chart.animateX(1500);//设置动画
    }


    /**
     * 设置折现图的样式
     * @param chart
     */
    private void setLineChart(LineChart chart) {
        chart.setDescription("Glan");
        chart.setDrawGridBackground(false);//设置网格背景
        chart.setScaleEnabled(false);//设置缩放
        chart.setDoubleTapToZoomEnabled(false);//设置双击不进行缩放

        //设置X轴
        XAxis xAxis = chart.getXAxis();
        xAxis.setPosition(XAxis.XAxisPosition.BOTTOM);//设置X轴的位置
//        xAxis.setTypeface(mTf);//设置字体
        xAxis.setDrawGridLines(false);
        xAxis.setDrawAxisLine(true);

        //获得左侧侧坐标轴
        YAxis leftAxis = chart.getAxisLeft();
//        leftAxis.setTypeface(mTf);
        leftAxis.setLabelCount(5);
//        leftAxis.setAxisLineWidth(1.5f);

        //设置右侧坐标轴
        YAxis rightAxis = chart.getAxisRight();
        rightAxis.setDrawAxisLine(false);//右侧坐标轴线
        rightAxis.setDrawLabels(false);//右侧坐标轴数组Lable
//        rightAxis.setTypeface(mTf);
//        rightAxis.setLabelCount(5);
//        rightAxis.setDrawGridLines(false);
    }

    private ArrayList<String> getXAxisShowLable() {
        ArrayList<String> m = new ArrayList<String>();
        m.add("Jan");
        m.add("Feb");
        m.add("Mar");
        m.add("Apr");
        m.add("May");
        m.add("Jun");
        m.add("Jul");
        m.add("Aug");
        m.add("Sep");
        m.add("Okt");
        m.add("Nov");
        m.add("Dec");
        return m;
    }
    /**
     * 饼形图的划分
     * @return
     */
    private ArrayList<String> getQuarters() {
        ArrayList<String> q = new ArrayList<String>();
        q.add("1st Quarter");
        q.add("2nd Quarter");
        q.add("3rd Quarter");
        q.add("4th Quarter");
        return q;
    }

}

```

