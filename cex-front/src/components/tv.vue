<template>
  <div
    id="tv_chart_container"
    style="width: 100%; height: 100%; margin: auto"
  ></div>
</template>
<script>
import Datafeeds from '@/assets/js/charting_library/datafeed/swaptrade.js';
import $ from '@/assets/js/jquery.min';
export default {
  components: {},
  props: ['symbol', 'interval', 'type'],
  data() {
    return {
      datafeed: null,
      dataParam: null
    };
  },
  created() {},
  computed: {
    theme() {
      return this.$store.state.theme;
    }
  },
  watch: {
    theme() {
      this.getKline(this.dataParam);
    }
  },
  methods: {
    /* getKline(data) {
      const { a, b, c, d } = data
      this.dataParam = data;
      this.datafeed = new Datafeeds.WebsockFeed(a, b, c, d)
      var that = this
      let config = {
        autosize: true,
        height: 400,
        symbol: that.symbol,
        interval: '5',
        timezone: 'Asia/Shanghai',
        toolbar_bg: '#161A1E',
        container_id: 'tv_chart_container',
        datafeed: that.datafeed,
        library_path: '/charting_library/',
        locale: 'en',
        debug: false,
        drawings_access: {
          type: 'black',
          tools: [{ name: 'Regression Trend' }],
        },
         hide_top_toolbar: true,
         hide_side_toolbar: false,
        disabled_features: [
          'header_resolutions',
          'timeframes_toolbar',
          'header_symbol_search',
          'header_chart_type',
          'header_compare',
          'header_undo_redo',
          'header_screenshot',
          'header_saveload',
          'volume_force_overlay',
          'widget_logo',
          'compare_symbol',
          'display_market_status',
          'go_to_date',
          'header_interval_dialog_button',
          'legend_context_menu',
          'show_hide_button_in_legend',
          'show_interval_dialog_on_key_press',
          'snapshot_trading_drawings',
          'symbol_info',
          'edit_buttons_in_legend',
          'context_menus',
          'control_bar',
          'border_around_the_chart',
        ],
        enabled_features: ["hide_left_toolbar_by_default"],
        studies_overrides: {
          'volume.volume.color.0': '#00b275',
          'volume.volume.color.1': '#f15057',
          'volume.volume.transparency': 25,
        },
        custom_css_url: 'bundles/common.css',
        supported_resolutions: ['1', '5', '15', '30', '60', '4H', '1D', '1W', '1M'],
        charts_storage_url: 'http://saveload.tradingview.com',
        charts_storage_api_version: '1.1',
        client_id: 'tradingview.com',
        user_id: 'public_user_id',
        overrides: {
          'paneProperties.background': '#161A1E',
          'paneProperties.vertGridProperties.color': 'rgba(0,0,0,.1)',
          'paneProperties.horzGridProperties.color': 'rgba(0,0,0,.1)',
          'scalesProperties.textColor': '#61688A',
          'mainSeriesProperties.candleStyle.upColor': '#00b275',
          'mainSeriesProperties.candleStyle.downColor': '#f15057',
          'mainSeriesProperties.candleStyle.drawBorder': false,
          'mainSeriesProperties.candleStyle.wickUpColor': '#589065',
          'mainSeriesProperties.candleStyle.wickDownColor': '#AE4E54',
          'paneProperties.legendProperties.showLegend': false,
          'mainSeriesProperties.areaStyle.color1': 'rgba(71, 78, 112, 0.5)',
          'mainSeriesProperties.areaStyle.color2': 'rgba(71, 78, 112, 0.5)',
          'mainSeriesProperties.areaStyle.linecolor': '#9194a4',
          volumePaneSize: 'small',
        },
        time_frames: [
          {
            text: '1min',
            resolution: '1',
            description: 'realtime',
            title: that.$t('swap.realtime'),
          },
          { text: '1min', resolution: '1', description: '1min' },
          { text: '5min', resolution: '5', description: '5min' },
          { text: '15min', resolution: '15', description: '15min' },
          { text: '30min', resolution: '30', description: '30min' },
          { text: '1hour', resolution: '60', description: '1hour', title: '1hour' },
          { text: '4hour', resolution: '240', description: '4hour', title: '4hour' },
          { text: '1day', resolution: '1D', description: '1day', title: '1day' },
          { text: '1week', resolution: '1W', description: '1week', title: '1week' },
          { text: '1mon', resolution: '1M', description: '1mon' },
        ],
      }
      let theme = localStorage.getItem('theme')
      console.log(theme);
      if (theme === 'white' || theme === null) {
        config.toolbar_bg = '#fff'
        config.custom_css_url = 'bundles/common_day.css'
        config.overrides['paneProperties.background'] = '#fff'
        config.overrides['mainSeriesProperties.candleStyle.upColor'] = '#a6d3a5'
        config.overrides['mainSeriesProperties.candleStyle.downColor'] = '#ffa5a6'
      }
      window.tvWidget = new TradingView.widget(config)
      require(['@/assets/js/charting_library/charting_library.min.js'], () => {
        var widget = (window.tvWidget = new TradingView.widget(config))
        widget.onChartReady(() => {
          widget.chart().executeActionById('drawingToolbarAction')
        })
        widget.onChartReady(()=> {
          widget
              .chart()
              .createStudy("Moving Average", false, false, [5], null, {
                "plot.color": "#EDEDED"
              });
          widget
              .chart()
              .createStudy("Moving Average", false, false, [10], null, {
                "plot.color": "#ffe000"
              });
          widget
              .chart()
              .createStudy("Moving Average", false, false, [30], null, {
                "plot.color": "#ce00ff"
              });
          widget
              .chart()
              .createStudy("Moving Average", false, false, [60], null, {
                "plot.color": "#00adff"
              });
          widget
              .createButton()
              .attr("title", "realtime")
              .on("click", function() {
                if ($(this).hasClass("selected")) return;
                $(this)
                    .addClass("selected")
                    .parent(".group")
                    .siblings(".group")
                    .find(".button.selected")
                    .removeClass("selected");
                widget.chart().setChartType(3);
                widget.setSymbol("", "1");
              })
              .append("<span>Time</span>");

          widget
              .createButton()
              .attr("title", "M1")
              .on("click", function() {
                if ($(this).hasClass("selected")) return;
                $(this)
                    .addClass("selected")
                    .parent(".group")
                    .siblings(".group")
                    .find(".button.selected")
                    .removeClass("selected");
                widget.chart().setChartType(1);
                widget.setSymbol("", "1");
              })
              .append("<span>M1</span>");

          widget
              .createButton()
              .attr("title", "M5")
              .on("click", function() {
                if ($(this).hasClass("selected")) return;
                $(this)
                    .addClass("selected")
                    .parent(".group")
                    .siblings(".group")
                    .find(".button.selected")
                    .removeClass("selected");
                widget.chart().setChartType(1);
                widget.setSymbol("", "5");
              })
              .append("<span>M5</span>")
              .addClass("selected");

          widget
              .createButton()
              .attr("title", "M15")
              .on("click", function() {
                if ($(this).hasClass("selected")) return;
                $(this)
                    .addClass("selected")
                    .parent(".group")
                    .siblings(".group")
                    .find(".button.selected")
                    .removeClass("selected");
                widget.chart().setChartType(1);
                widget.setSymbol("", "15");
              })
              .append("<span>M15</span>");

          widget
              .createButton()
              .attr("title", "M30")
              .on("click", function() {
                if ($(this).hasClass("selected")) return;
                $(this)
                    .addClass("selected")
                    .parent(".group")
                    .siblings(".group")
                    .find(".button.selected")
                    .removeClass("selected");
                widget.chart().setChartType(1);
                widget.setSymbol("", "30");
              })
              .append("<span>M30</span>");

          widget
              .createButton()
              .attr("title", "H1")
              .on("click", function() {
                if ($(this).hasClass("selected")) return;
                $(this)
                    .addClass("selected")
                    .parent(".group")
                    .siblings(".group")
                    .find(".button.selected")
                    .removeClass("selected");
                widget.chart().setChartType(1);
                widget.setSymbol("", "60");
              })
              .append("<span>H1</span>");

          widget
              .createButton()
              .attr("title", "H4")
              .on("click", function() {
                if ($(this).hasClass("selected")) return;
                $(this)
                    .addClass("selected")
                    .parent(".group")
                    .siblings(".group")
                    .find(".button.selected")
                    .removeClass("selected");
                widget.chart().setChartType(1);
                widget.setSymbol("", "240");
              })
              .append("<span>H4</span>");

          widget
              .createButton()
              .attr("title", "D1")
              .on("click", function() {
                if ($(this).hasClass("selected")) return;
                $(this)
                    .addClass("selected")
                    .parent(".group")
                    .siblings(".group")
                    .find(".button.selected")
                    .removeClass("selected");
                widget.chart().setChartType(1);
                widget.setSymbol("", "1D");
              })
              .append("<span>D1</span>");
        });
      })
    }, */

    // 【获取K线数据并初始化 TradingView 图表】
    getKline(data) {
      // 解构传入的WebSocket连接参数（a、b、c、d 通常是WebSocket的地址、订阅频道等配置）
      const { a, b, c, d } = data;
      // 存储数据参数，供后续使用
      this.dataParam = data;
      // 初始化数据馈送（Datafeed）：用于和 TradingView 通信，传递K线、行情等数据
      // 这里使用了自定义的 WebsockFeed 类，基于WebSocket实现数据推送
      this.datafeed = new Datafeeds.WebsockFeed(a, b, c, d);

      // 保存this上下文，避免箭头函数外的作用域丢失
      let that = this;

      // 【TradingView Widget 配置项】：定义图表的样式、功能、数据源等
      let config = {
        // 自动适配容器大小
        autosize: true,
        // 图表默认高度
        height: 400,
        // 交易对符号（由业务逻辑传入，如 BTC/USDT）
        symbol: that.symbol,
        // 初始时间周期（5分钟，对应 intervalMap 中的映射）
        interval: '5',
        // 时区（亚洲/上海）
        timezone: 'Asia/Shanghai',
        // 工具栏背景色
        toolbar_bg: '#161A1E',
        // 图表容器的ID（与模板中div的ID对应）
        container_id: 'tv_chart_container',
        // 数据馈送实例（用于传递K线数据）
        datafeed: that.datafeed,
        // TradingView 库的路径（需提前将 charting_library 资源部署到该路径）
        library_path: '/charting_library/',
        // 语言（英文）
        locale: 'en',
        // 是否开启调试模式
        debug: false,
        // 绘图工具权限：仅允许“回归趋势线”工具
        drawings_access: {
          type: 'black',
          tools: [{ name: 'Regression Trend' }]
        },
        // 隐藏顶部工具栏
        hide_top_toolbar: true,
        // 不隐藏侧边工具栏
        hide_side_toolbar: false,
        // 禁用的功能列表：隐藏不需要的UI组件（如周期选择、截图、保存等）
        disabled_features: [
          'header_resolutions',
          'timeframes_toolbar',
          'header_symbol_search',
          'header_chart_type',
          'header_compare',
          'header_undo_redo',
          'header_screenshot',
          'header_saveload',
          'volume_force_overlay',
          'widget_logo',
          'compare_symbol',
          'display_market_status',
          'go_to_date',
          'header_interval_dialog_button',
          'legend_context_menu',
          'show_hide_button_in_legend',
          'show_interval_dialog_on_key_press',
          'snapshot_trading_drawings',
          'symbol_info',
          'edit_buttons_in_legend',
          'context_menus',
          'control_bar',
          'border_around_the_chart'
        ],
        // 启用的功能：默认隐藏左侧工具栏
        enabled_features: ['hide_left_toolbar_by_default'],
        // 指标样式覆盖：自定义成交量、均线等的颜色
        studies_overrides: {
          'volume.volume.color.0': '#00b275', // 上涨成交量颜色
          'volume.volume.color.1': '#f15057', // 下跌成交量颜色
          'volume.volume.transparency': 25 // 成交量透明度
        },
        // 自定义CSS路径：用于覆盖图表样式
        custom_css_url: 'bundles/common.css',
        // 支持的时间周期（与 time_frames 对应）
        supported_resolutions: [
          '1',
          '5',
          '15',
          '30',
          '60',
          '4H',
          '1D',
          '1W',
          '1M'
        ],
        // 图表保存/加载的服务地址（TradingView 官方服务）
        charts_storage_url: 'http://saveload.tradingview.com',
        charts_storage_api_version: '1.1',
        // 客户端ID和用户ID（公共用户，无需登录）
        client_id: 'tradingview.com',
        user_id: 'public_user_id',
        // 图表样式覆盖：自定义背景、网格、蜡烛图颜色等
        overrides: {
          'paneProperties.background': '#161A1E', // 面板背景色
          'paneProperties.vertGridProperties.color': 'rgba(0,0,0,.1)', // 垂直网格颜色
          'paneProperties.horzGridProperties.color': 'rgba(0,0,0,.1)', // 水平网格颜色
          'scalesProperties.textColor': '#61688A', // 刻度文字颜色
          'mainSeriesProperties.candleStyle.upColor': '#00b275', // 上涨蜡烛颜色
          'mainSeriesProperties.candleStyle.downColor': '#f15057', // 下跌蜡烛颜色
          'mainSeriesProperties.candleStyle.drawBorder': false, // 蜡烛图是否绘制边框
          'mainSeriesProperties.candleStyle.wickUpColor': '#589065', // 上涨影线颜色
          'mainSeriesProperties.candleStyle.wickDownColor': '#AE4E54', // 下跌影线颜色
          'paneProperties.legendProperties.showLegend': false, // 是否显示图例
          'mainSeriesProperties.areaStyle.color1': 'rgba(71, 78, 112, 0.5)', // 面积图颜色1
          'mainSeriesProperties.areaStyle.color2': 'rgba(71, 78, 112, 0.5)', // 面积图颜色2
          'mainSeriesProperties.areaStyle.linecolor': '#9194a4', // 面积图线条颜色
          volumePaneSize: 'small' // 成交量面板大小
        },
        // 时间周期列表：定义前端展示的周期选项及对应分辨率
        time_frames: [
          {
            text: '1min',
            resolution: '1',
            description: 'realtime',
            title: that.$t('swap.realtime')
          },
          { text: '1min', resolution: '1', description: '1min' },
          { text: '5min', resolution: '5', description: '5min' },
          { text: '15min', resolution: '15', description: '15min' },
          { text: '30min', resolution: '30', description: '30min' },
          {
            text: '1hour',
            resolution: '60',
            description: '1hour',
            title: '1hour'
          },
          {
            text: '4hour',
            resolution: '240',
            description: '4hour',
            title: '4hour'
          },
          {
            text: '1day',
            resolution: '1D',
            description: '1day',
            title: '1day'
          },
          {
            text: '1week',
            resolution: '1W',
            description: '1week',
            title: '1week'
          },
          { text: '1mon', resolution: '1M', description: '1mon' }
        ]
      };

      // 【主题切换逻辑】：根据本地存储的主题（亮色/暗色）修改图表样式
      let theme = localStorage.getItem('theme');
      console.log(theme);
      if (theme === 'white' || theme === null) {
        // 亮色主题配置
        config.toolbar_bg = '#fff'; // 工具栏背景色改为白色
        config.custom_css_url = 'bundles/common_day.css'; // 加载亮色主题CSS
        // 覆盖面板背景和蜡烛图颜色
        config.overrides['paneProperties.background'] = '#fff';
        config.overrides['mainSeriesProperties.candleStyle.upColor'] =
          '#a6d3a5';
        config.overrides['mainSeriesProperties.candleStyle.downColor'] =
          '#ffa5a6';
      }

      // 【异步加载 TradingView 库并初始化 Widget】
      // 动态导入 charting_library.min.js（确保资源已部署）
      require(['@/assets/js/charting_library/charting_library.min.js'], () => {
        // 初始化 TradingView Widget，传入配置项
        var widget = (window.tvWidget = new TradingView.widget(config));

        // 【图表加载完成回调】：图表渲染完成后执行的逻辑
        widget.onChartReady(() => {
          // 执行“绘图工具栏”动作（显示绘图工具）
          widget.chart().executeActionById('drawingToolbarAction');
        });

        // 【图表加载完成后初始化指标和自定义按钮】
        widget.onChartReady(() => {
          // 1. 创建均线指标（Moving Average）
          widget
            .chart()
            .createStudy('Moving Average', false, false, [5], null, {
              'plot.color': '#EDEDED' // 5日均线颜色
            });
          widget
            .chart()
            .createStudy('Moving Average', false, false, [10], null, {
              'plot.color': '#ffe000' // 10日均线颜色
            });
          widget
            .chart()
            .createStudy('Moving Average', false, false, [30], null, {
              'plot.color': '#ce00ff' // 30日均线颜色
            });
          widget
            .chart()
            .createStudy('Moving Average', false, false, [60], null, {
              'plot.color': '#00adff' // 60日均线颜色
            });

          // 2. 创建自定义周期切换按钮
          // 实时周期按钮
          widget
            .createButton()
            .attr('title', 'realtime')
            .on('click', function () {
              if ($(this).hasClass('selected')) return;
              $(this)
                .addClass('selected')
                .parent('.group')
                .siblings('.group')
                .find('.button.selected')
                .removeClass('selected');
              widget.chart().setChartType(3); // 设置图表类型（实时）
              widget.setSymbol('', '1'); // 设置周期（1分钟）
            })
            .append('<span>Time</span>');

          // 1分钟周期按钮
          widget
            .createButton()
            .attr('title', 'M1')
            .on('click', function () {
              if ($(this).hasClass('selected')) return;
              $(this)
                .addClass('selected')
                .parent('.group')
                .siblings('.group')
                .find('.button.selected')
                .removeClass('selected');
              widget.chart().setChartType(1); // 设置图表类型（K线）
              widget.setSymbol('', '1'); // 设置周期（1分钟）
            })
            .append('<span>M1</span>')
            .addClass('selected'); // 默认选中

          // 5分钟周期按钮
          widget
            .createButton()
            .attr('title', 'M5')
            .on('click', function () {
              if ($(this).hasClass('selected')) return;
              $(this)
                .addClass('selected')
                .parent('.group')
                .siblings('.group')
                .find('.button.selected')
                .removeClass('selected');
              widget.chart().setChartType(1);
              widget.setSymbol('', '5'); // 5分钟周期
            })
            .append('<span>M5</span>');

          // 15分钟周期按钮
          widget
            .createButton()
            .attr('title', 'M15')
            .on('click', function () {
              if ($(this).hasClass('selected')) return;
              $(this)
                .addClass('selected')
                .parent('.group')
                .siblings('.group')
                .find('.button.selected')
                .removeClass('selected');
              widget.chart().setChartType(1);
              widget.setSymbol('', '15'); // 15分钟周期
            })
            .append('<span>M15</span>');

          // 30分钟周期按钮
          widget
            .createButton()
            .attr('title', 'M30')
            .on('click', function () {
              if ($(this).hasClass('selected')) return;
              $(this)
                .addClass('selected')
                .parent('.group')
                .siblings('.group')
                .find('.button.selected')
                .removeClass('selected');
              widget.chart().setChartType(1);
              widget.setSymbol('', '30'); // 30分钟周期
            })
            .append('<span>M30</span>');

          // 1小时周期按钮
          widget
            .createButton()
            .attr('title', 'H1')
            .on('click', function () {
              if ($(this).hasClass('selected')) return;
              $(this)
                .addClass('selected')
                .parent('.group')
                .siblings('.group')
                .find('.button.selected')
                .removeClass('selected');
              widget.chart().setChartType(1);
              widget.setSymbol('', '60'); // 1小时周期
            })
            .append('<span>H1</span>');

          // 4小时周期按钮
          widget
            .createButton()
            .attr('title', 'H4')
            .on('click', function () {
              if ($(this).hasClass('selected')) return;
              $(this)
                .addClass('selected')
                .parent('.group')
                .siblings('.group')
                .find('.button.selected')
                .removeClass('selected');
              widget.chart().setChartType(1);
              widget.setSymbol('', '240'); // 4小时周期
            })
            .append('<span>H4</span>');

          // 1天周期按钮
          widget
            .createButton()
            .attr('title', 'D1')
            .on('click', function () {
              if ($(this).hasClass('selected')) return;
              $(this)
                .addClass('selected')
                .parent('.group')
                .siblings('.group')
                .find('.button.selected')
                .removeClass('selected');
              widget.chart().setChartType(1);
              widget.setSymbol('', '1D'); // 1天周期
            })
            .append('<span>D1</span>');
        });
      });
    }
  }
};
</script>
