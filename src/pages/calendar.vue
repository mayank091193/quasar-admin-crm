<template>
  <q-page>
    <q-toolbar>
      <q-btn flat dense label="Today" class="q-mr-md bg-blue text-white" @click="setToday()"></q-btn>
      <q-btn flat dense round icon="keyboard_arrow_left" class="q-mr-sm bg-blue text-white" @click="onPrev()"></q-btn>
      <q-btn flat dense round icon="keyboard_arrow_right" class="bg-blue text-white" @click="onNext"></q-btn>
      <span class="q-mr-xl q-toolbar__title nowrap">{{ title() }}</span>
      <q-select
        outlined
        dense
        emit-value
        map-options
        label="Change theme"
        v-model="theme"
        :options="themesList"
        class="float-right bg-white"
        style="width:130px"
      ></q-select>
    </q-toolbar>
    <div style="width: 100%;" class="bg-white">
      <q-calendar
        v-model="selectedDate"
        :view="view_name"
        ref="calendar"
        locale="en-us"
        enable-theme
        :theme="theme"
        style="height: 550px;border:1px solid #e0e0e0"
      >
        <template #day="{ date }">
          <template v-for="(event, index) in getEvents(date)">
            <q-badge
              :key="index"
              style="width: 100%; cursor: pointer; height: 16px; max-height: 16px"
              class="q-event"
              :class="badgeClasses(event, 'day')"
              :style="badgeStyles(event, 'day')"
            >
              <q-icon v-if="event.icon" :name="event.icon" class="q-mr-xs"></q-icon>
              <span class="ellipsis">{{ event.title }}</span>
            </q-badge>
          </template>
        </template>
      </q-calendar>
    </div>
  </q-page>
</template>

<script>
    // normally you would not import "all" of QCalendar, but is needed for this example to work with UMD (codepen)
    import QCalendar from '@quasar/quasar-ui-qcalendar' // ui is aliased from '@quasar/quasar-ui-qcalendar'
    const CURRENT_DAY = new Date()

    const reRGBA = /^\s*rgb(a)?\s*\((\s*(\d+)\s*,\s*?){2}(\d+)\s*,?\s*([01]?\.?\d*?)?\s*\)\s*$/

    function textToRgb(color) {
        if (typeof color !== 'string') {
            throw new TypeError('Expected a string')
        }
        const m = reRGBA.exec(color)
        if (m) {
            const rgb = {
                r: Math.min(255, parseInt(m[2], 10)),
                g: Math.min(255, parseInt(m[3], 10)),
                b: Math.min(255, parseInt(m[4], 10))
            }
            if (m[1]) {
                rgb.a = Math.min(1, parseFloat(m[5]))
            }
            return rgb
        }
        return hexToRgb(color)
    }

    function hexToRgb(hex) {
        if (typeof hex !== 'string') {
            throw new TypeError('Expected a string')
        }
        hex = hex.replace(/^#/, '')
        if (hex.length === 3) {
            hex = hex[0] + hex[0] + hex[1] + hex[1] + hex[2] + hex[2]
        } else if (hex.length === 4) {
            hex = hex[0] + hex[0] + hex[1] + hex[1] + hex[2] + hex[2] + hex[3] + hex[3]
        }
        const num = parseInt(hex, 16)
        return hex.length > 6
            ? {r: num >> 24 & 255, g: num >> 16 & 255, b: num >> 8 & 255, a: Math.round((num & 255) / 2.55)}
            : {r: num >> 16, g: num >> 8 & 255, b: num & 255}
    }

    function luminosity(color) {
        if (typeof color !== 'string' && (!color || color.r === void 0)) {
            throw new TypeError('Expected a string or a {r, g, b} object as color')
        }
        const
            rgb = typeof color === 'string' ? textToRgb(color) : color,
            r = rgb.r / 255,
            g = rgb.g / 255,
            b = rgb.b / 255,
            R = r <= 0.03928 ? r / 12.92 : Math.pow((r + 0.055) / 1.055, 2.4),
            G = g <= 0.03928 ? g / 12.92 : Math.pow((g + 0.055) / 1.055, 2.4),
            B = b <= 0.03928 ? b / 12.92 : Math.pow((b + 0.055) / 1.055, 2.4)
        return 0.2126 * R + 0.7152 * G + 0.0722 * B
    }

    export default {
        data() {
            return {
                selectedDate: (new Date()).getFullYear() + '-' + ((new Date()).getMonth() + 1) + '-' + (new Date()).getDate(),
                titleFormatter: null,
                dateFormatter: null,
                view_name: 'month',
                events: [
                    {
                        title: 'Conference call',
                        date: "2020-04-14",
                        bgcolor: 'orange'
                    },
                    {
                        title: 'Conference call with Jack',
                        date: "2020-04-24",
                        bgcolor: 'orange'
                    },
                    {
                        title: 'Conference call',
                        date: "2020-05-15",
                        bgcolor: 'orange'
                    },
                    {
                        title: 'Conference call with Jack',
                        date: "2020-04-03",
                        bgcolor: 'orange'
                    },
                    {
                        title: 'Give feedback',
                        details: 'Buy a nice present',
                        date: "2020-04-20",
                        bgcolor: 'green',
                        icon: 'fas fa-birthday-cake'
                    },
                    {
                        title: 'Meeting',
                        details: 'Time to pitch my idea to the company',
                        date: "2020-04-12",
                        bgcolor: 'blue',
                        icon: 'fas fa-handshake'
                    },
                    {
                        title: 'Meeting',
                        details: 'Time to pitch my idea to the company',
                        date: "2020-05-12",
                        bgcolor: 'blue',
                        icon: 'fas fa-handshake'
                    },
                    {
                        title: 'Lunch',
                        details: 'Company is paying!',
                        date: "2020-05-14",
                        bgcolor: 'teal',
                        icon: 'fas fa-hamburger'
                    },
                    {
                        title: 'Leave',
                        details: 'Always a nice chat with mom',
                        date: "2020-04-28",
                        bgcolor: 'blue-grey',
                        icon: 'fas fa-car'
                    },
                    {
                        title: 'Leave',
                        details: 'Always a nice chat with mom',
                        date: "2020-04-26",
                        bgcolor: 'blue-grey',
                        icon: 'fas fa-car'
                    },
                    {
                        title: 'Leave',
                        details: 'Always a nice chat with mom',
                        date: "2020-05-25",
                        bgcolor: 'blue-grey',
                        icon: 'fas fa-car'
                    },
                    {
                        title: 'Conference call',
                        details: 'Teaching Javascript 101',
                        date: "2020-05-06",
                        bgcolor: 'blue',
                        icon: 'fas fa-chalkboard-teacher'
                    },
                    {
                        title: 'Conference call',
                        details: 'Teaching Javascript 101',
                        date: "2020-04-07",
                        bgcolor: 'blue',
                        icon: 'fas fa-chalkboard-teacher'
                    },
                    {
                        title: 'Rowing',
                        details: 'Time for some weekend R&R',
                        date: "2020-04-5",
                        bgcolor: 'purple',
                        icon: 'rowing',
                        days: 1
                    },
                    {
                        title: 'Meeting',
                        details: 'Trails and hikes, going camping! Don\'t forget to bring bear spray!',
                        date: "2020-04-09",
                        bgcolor: 'blue',
                        icon: 'fas fa-handshake',
                        days: 2
                    }
                ],
                theme: {
                    name: 'default'
                },
                themes: [
                    {
                        name: 'default'
                    },
                    {
                        name: 'dark',
                        // body
                        colorBody: 'grey-2',
                        backgroundBody: 'blue-grey-8',
                        colorBodyOutside: 'grey-6',
                        backgroundBodyOutside: 'blue-grey-9',
                        colorBodyPast: 'grey-11',
                        backgroundBodyPast: 'blue-grey-10',
                        colorBodyCurrent: 'blue-grey-2',
                        backgroundBodyCurrent: 'blue-grey-10',
                        colorBodyFuture: 'blue-grey-2',
                        backgroundBodyFuture: 'blue-grey-10',
                        // header - weekly only
                        colorHeader: 'blue-grey-2',
                        backgroundHeader: 'blue-grey-10',
                        colorHeaderOutside: 'blue-grey-2',
                        backgroundHeaderOutside: 'blue-grey-10',
                        // header - for daily only
                        colorHeaderPast: 'grey-11',
                        backgroundHeaderPast: 'blue-grey-10',
                        colorHeaderCurrent: 'blue-grey-2',
                        backgroundHeaderCurrent: 'blue-grey-10',
                        colorHeaderFuture: 'blue-grey-2',
                        backgroundHeaderFuture: 'blue-grey-10',
                        // work week - monthly only
                        colorWorkWeekPast: 'blue-grey-8',
                        backgroundWorkWeekPast: 'blue-grey-6',
                        colorWorkWeekCurrent: 'orange-4',
                        backgroundWorkWeekCurrent: 'blue-grey-10',
                        colorWorkWeekFuture: 'yellow-4',
                        backgroundWorkWeekFuture: 'blue-grey-10',
                        // label
                        colorDayLabelOutside: 'blue-grey-2',
                        backgroundDayLabelOutside: 'blue-grey-9',
                        colorDayLabelPast: 'yellow-4',
                        backgroundDayLabelPast: 'blue-grey-10',
                        colorDayLabelCurrent: 'orange-4',
                        backgroundDayLabelCurrent: 'blue-grey-10',
                        colorDayLabelFuture: 'yellow-4',
                        backgroundDayLabelFuture: 'blue-grey-10',
                        // interval body
                        colorIntervalHeader: 'grey-2',
                        backgroundIntervalHeader: 'blue-grey-10',
                        colorIntervalBody: 'grey-2',
                        backgroundIntervalBody: 'blue-grey-10',
                        colorIntervalText: 'grey-2',
                        backgroundIntervalText: 'blue-grey-10',
                        // scheduler body
                        colorSchedulerHeader: 'grey-2',
                        backgroundSchedulerHeader: 'blue-grey-10',
                        colorSchedulerBody: 'grey-2',
                        backgroundSchedulerBody: 'blue-grey-10',
                        colorSchedulerText: 'grey-2',
                        backgroundSchedulerText: 'blue-grey-10'
                    },
                    {
                        name: 'teal',
                        // body
                        colorBody: 'grey-2',
                        backgroundBody: 'teal-8',
                        colorBodyOutside: 'grey-6',
                        backgroundBodyOutside: 'teal-9',
                        colorBodyPast: 'grey-11',
                        backgroundBodyPast: 'teal-10',
                        colorBodyCurrent: 'teal-2',
                        backgroundBodyCurrent: 'teal-10',
                        colorBodyFuture: 'teal-2',
                        backgroundBodyFuture: 'teal-10',
                        // header - weekly only
                        colorHeader: 'teal-2',
                        backgroundHeader: 'teal-10',
                        colorHeaderOutside: 'teal-2',
                        backgroundHeaderOutside: 'teal-10',
                        // header - for daily only
                        colorHeaderPast: 'grey-11',
                        backgroundHeaderPast: 'teal-10',
                        colorHeaderCurrent: 'teal-2',
                        backgroundHeaderCurrent: 'teal-10',
                        colorHeaderFuture: 'teal-2',
                        backgroundHeaderFuture: 'teal-10',
                        // work week - monthly only
                        colorWorkWeekPast: 'teal-8',
                        backgroundWorkWeekPast: 'teal-6',
                        colorWorkWeekCurrent: 'orange-4',
                        backgroundWorkWeekCurrent: 'teal-10',
                        colorWorkWeekFuture: 'yellow-4',
                        backgroundWorkWeekFuture: 'teal-10',
                        // label
                        colorDayLabelOutside: 'teal-2',
                        backgroundDayLabelOutside: 'teal-9',
                        colorDayLabelPast: 'yellow-4',
                        backgroundDayLabelPast: 'teal-10',
                        colorDayLabelCurrent: 'orange-4',
                        backgroundDayLabelCurrent: 'teal-10',
                        colorDayLabelFuture: 'yellow-4',
                        backgroundDayLabelFuture: 'teal-10',
                        // interval body
                        colorIntervalHeader: 'grey-2',
                        backgroundIntervalHeader: 'teal-10',
                        colorIntervalBody: 'grey-2',
                        backgroundIntervalBody: 'teal-10',
                        colorIntervalText: 'grey-2',
                        backgroundIntervalText: 'teal-10',
                        // scheduler body
                        colorSchedulerHeader: 'grey-2',
                        backgroundSchedulerHeader: 'teal-10',
                        colorSchedulerBody: 'grey-2',
                        backgroundSchedulerBody: 'teal-10',
                        colorSchedulerText: 'grey-2',
                        backgroundSchedulerText: 'teal-10'
                    },
                    {
                        name: 'brown',
                        // body
                        colorBody: 'brown-2',
                        backgroundBody: 'brown-8',
                        colorBodyOutside: 'grey-6',
                        backgroundBodyOutside: 'brown-9',
                        colorBodyPast: 'grey-11',
                        backgroundBodyPast: 'brown-10',
                        colorBodyCurrent: 'brown-2',
                        backgroundBodyCurrent: 'brown-10',
                        colorBodyFuture: 'brown-2',
                        backgroundBodyFuture: 'brown-10',
                        // header - weekly only
                        colorHeader: 'brown-2',
                        backgroundHeader: 'brown-10',
                        colorHeaderOutside: 'brown-2',
                        backgroundHeaderOutside: 'brown-10',
                        // header - for daily only
                        colorHeaderPast: 'grey-11',
                        backgroundHeaderPast: 'brown-10',
                        colorHeaderCurrent: 'brown-2',
                        backgroundHeaderCurrent: 'brown-10',
                        colorHeaderFuture: 'brown-2',
                        backgroundHeaderFuture: 'brown-10',
                        // work week - monthly only
                        colorWorkWeekPast: 'brown-8',
                        backgroundWorkWeekPast: 'brown-6',
                        colorWorkWeekCurrent: 'orange-4',
                        backgroundWorkWeekCurrent: 'brown-10',
                        colorWorkWeekFuture: 'yellow-4',
                        backgroundWorkWeekFuture: 'brown-10',
                        // label
                        colorDayLabelOutside: 'brown-2',
                        backgroundDayLabelOutside: 'brown-9',
                        colorDayLabelPast: 'yellow-4',
                        backgroundDayLabelPast: 'brown-10',
                        colorDayLabelCurrent: 'orange-4',
                        backgroundDayLabelCurrent: 'brown-10',
                        colorDayLabelFuture: 'yellow-4',
                        backgroundDayLabelFuture: 'brown-10',
                        // interval body
                        colorIntervalHeader: 'grey-2',
                        backgroundIntervalHeader: 'brown-10',
                        colorIntervalBody: 'grey-2',
                        backgroundIntervalBody: 'brown-10',
                        colorIntervalText: 'grey-2',
                        backgroundIntervalText: 'brown-10',
                        // scheduler body
                        colorSchedulerHeader: 'grey-2',
                        backgroundSchedulerHeader: 'brown-10',
                        colorSchedulerBody: 'grey-2',
                        backgroundSchedulerBody: 'brown-10',
                        colorSchedulerText: 'grey-2',
                        backgroundSchedulerText: 'brown-10'
                    },
                    {
                        name: 'deep purple',
                        // body
                        colorBody: 'grey-2',
                        backgroundBody: 'deep-purple-8',
                        colorBodyOutside: 'grey-6',
                        backgroundBodyOutside: 'deep-purple-9',
                        colorBodyPast: 'grey-11',
                        backgroundBodyPast: 'deep-purple-10',
                        colorBodyCurrent: 'deep-purple-2',
                        backgroundBodyCurrent: 'deep-purple-10',
                        colorBodyFuture: 'deep-purple-2',
                        backgroundBodyFuture: 'deep-purple-10',
                        // header - weekly only
                        colorHeader: 'deep-purple-2',
                        backgroundHeader: 'deep-purple-10',
                        colorHeaderOutside: 'deep-purple-2',
                        backgroundHeaderOutside: 'deep-purple-10',
                        // header - for daily only
                        colorHeaderPast: 'grey-11',
                        backgroundHeaderPast: 'deep-purple-10',
                        colorHeaderCurrent: 'deep-purple-2',
                        backgroundHeaderCurrent: 'deep-purple-10',
                        colorHeaderFuture: 'deep-purple-2',
                        backgroundHeaderFuture: 'deep-purple-10',
                        // work week - monthly only
                        colorWorkWeekPast: 'deep-purple-8',
                        backgroundWorkWeekPast: 'deep-purple-6',
                        colorWorkWeekCurrent: 'orange-4',
                        backgroundWorkWeekCurrent: 'deep-purple-10',
                        colorWorkWeekFuture: 'yellow-4',
                        backgroundWorkWeekFuture: 'deep-purple-10',
                        // label
                        colorDayLabelOutside: 'deep-purple-2',
                        backgroundDayLabelOutside: 'deep-purple-9',
                        colorDayLabelPast: 'yellow-4',
                        backgroundDayLabelPast: 'deep-purple-10',
                        colorDayLabelCurrent: 'orange-4',
                        backgroundDayLabelCurrent: 'deep-purple-10',
                        colorDayLabelFuture: 'yellow-4',
                        backgroundDayLabelFuture: 'deep-purple-10',
                        // interval body
                        colorIntervalHeader: 'grey-2',
                        backgroundIntervalHeader: 'deep-purple-10',
                        colorIntervalBody: 'grey-2',
                        backgroundIntervalBody: 'deep-purple-10',
                        colorIntervalText: 'grey-2',
                        backgroundIntervalText: 'deep-purple-10',
                        // scheduler body
                        colorSchedulerHeader: 'grey-2',
                        backgroundSchedulerHeader: 'deep-purple-10',
                        colorSchedulerBody: 'grey-2',
                        backgroundSchedulerBody: 'deep-purple-10',
                        colorSchedulerText: 'grey-2',
                        backgroundSchedulerText: 'deep-purple-10'
                    },
                    {
                        name: 'indigo',
                        // body
                        colorBody: 'grey-2',
                        backgroundBody: 'indigo-8',
                        colorBodyOutside: 'grey-6',
                        backgroundBodyOutside: 'indigo-9',
                        colorBodyPast: 'grey-11',
                        backgroundBodyPast: 'indigo-10',
                        colorBodyCurrent: 'indigo-2',
                        backgroundBodyCurrent: 'indigo-10',
                        colorBodyFuture: 'indigo-2',
                        backgroundBodyFuture: 'indigo-10',
                        // header - weekly only
                        colorHeader: 'indigo-2',
                        backgroundHeader: 'indigo-10',
                        colorHeaderOutside: 'indigo-2',
                        backgroundHeaderOutside: 'indigo-10',
                        // header - for daily only
                        colorHeaderPast: 'grey-11',
                        backgroundHeaderPast: 'indigo-10',
                        colorHeaderCurrent: 'indigo-2',
                        backgroundHeaderCurrent: 'indigo-10',
                        colorHeaderFuture: 'indigo-2',
                        backgroundHeaderFuture: 'indigo-10',
                        // work week - monthly only
                        colorWorkWeekPast: 'indigo-8',
                        backgroundWorkWeekPast: 'indigo-6',
                        colorWorkWeekCurrent: 'orange-4',
                        backgroundWorkWeekCurrent: 'indigo-10',
                        colorWorkWeekFuture: 'yellow-4',
                        backgroundWorkWeekFuture: 'indigo-10',
                        // label
                        colorDayLabelOutside: 'indigo-2',
                        backgroundDayLabelOutside: 'indigo-9',
                        colorDayLabelPast: 'yellow-4',
                        backgroundDayLabelPast: 'indigo-10',
                        colorDayLabelCurrent: 'orange-4',
                        backgroundDayLabelCurrent: 'indigo-10',
                        colorDayLabelFuture: 'yellow-4',
                        backgroundDayLabelFuture: 'indigo-10',
                        // interval body
                        colorIntervalHeader: 'grey-2',
                        backgroundIntervalHeader: 'indigo-10',
                        colorIntervalBody: 'grey-2',
                        backgroundIntervalBody: 'indigo-10',
                        colorIntervalText: 'grey-2',
                        backgroundIntervalText: 'indigo-10',
                        // scheduler body
                        colorSchedulerHeader: 'grey-2',
                        backgroundSchedulerHeader: 'indigo-10',
                        colorSchedulerBody: 'grey-2',
                        backgroundSchedulerBody: 'indigo-10',
                        colorSchedulerText: 'grey-2',
                        backgroundSchedulerText: 'indigo-10'
                    },
                    {
                        name: 'blue',
                        // body
                        colorBody: 'grey-2',
                        backgroundBody: 'blue-8',
                        colorBodyOutside: 'grey-6',
                        backgroundBodyOutside: 'blue-9',
                        colorBodyPast: 'grey-11',
                        backgroundBodyPast: 'blue-10',
                        colorBodyCurrent: 'blue-2',
                        backgroundBodyCurrent: 'blue-10',
                        colorBodyFuture: 'blue-2',
                        backgroundBodyFuture: 'blue-10',
                        // header - weekly only
                        colorHeader: 'blue-2',
                        backgroundHeader: 'blue-10',
                        colorHeaderOutside: 'blue-2',
                        backgroundHeaderOutside: 'blue-10',
                        // header - for daily only
                        colorHeaderPast: 'grey-11',
                        backgroundHeaderPast: 'blue-10',
                        colorHeaderCurrent: 'blue-2',
                        backgroundHeaderCurrent: 'blue-10',
                        colorHeaderFuture: 'blue-2',
                        backgroundHeaderFuture: 'blue-10',
                        // work week - monthly only
                        colorWorkWeekPast: 'blue-8',
                        backgroundWorkWeekPast: 'blue-6',
                        colorWorkWeekCurrent: 'orange-4',
                        backgroundWorkWeekCurrent: 'blue-10',
                        colorWorkWeekFuture: 'yellow-4',
                        backgroundWorkWeekFuture: 'blue-10',
                        // label
                        colorDayLabelOutside: 'blue-2',
                        backgroundDayLabelOutside: 'blue-9',
                        colorDayLabelPast: 'yellow-4',
                        backgroundDayLabelPast: 'blue-10',
                        colorDayLabelCurrent: 'orange-4',
                        backgroundDayLabelCurrent: 'blue-10',
                        colorDayLabelFuture: 'yellow-4',
                        backgroundDayLabelFuture: 'blue-10',
                        // interval body
                        colorIntervalHeader: 'grey-2',
                        backgroundIntervalHeader: 'blue-10',
                        colorIntervalBody: 'grey-2',
                        backgroundIntervalBody: 'blue-10',
                        colorIntervalText: 'grey-2',
                        backgroundIntervalText: 'blue-10',
                        // scheduler body
                        colorSchedulerHeader: 'grey-2',
                        backgroundSchedulerHeader: 'blue-10',
                        colorSchedulerBody: 'grey-2',
                        backgroundSchedulerBody: 'blue-10',
                        colorSchedulerText: 'grey-2',
                        backgroundSchedulerText: 'blue-10'
                    }
                ]
            }
        },
        created() {
            this.updateFormatters();
        },
        computed: {
            themesList() {
                const list = []
                this.themes.forEach((theme) => {
                    list.push({
                        label: theme.name,
                        value: {...theme}
                    })
                })
                return list
            }
        },
        methods: {
            onPrev() {
                this.$refs.calendar.prev();
            },
            onNext() {
                this.$refs.calendar.next();
            },
            setToday() {
                this.selectedDate = (new Date()).getFullYear() + '-' + ((new Date()).getMonth() + 1) + '-' + (new Date()).getDate();
            },
            title() {
                if (this.selectedDate) {
                    const date = new Date(this.selectedDate)
                    return this.titleFormatter.format(date)
                }
                return ''
            },
            isCssColor(color) {
                return !!color && !!color.match(/^(#|(rgb|hsl)a?\()/)
            },
            badgeClasses(event, type) {
                const cssColor = this.isCssColor(event.bgcolor)
                const isHeader = type === 'header'
                return {
                    [`text-white bg-${event.bgcolor}`]: !cssColor,
                    'full-width': !isHeader && (!event.side || event.side === 'full'),
                    'left-side': !isHeader && event.side === 'left',
                    'right-side': !isHeader && event.side === 'right'
                }
            },
            badgeStyles(event, type, timeStartPos, timeDurationHeight) {
                const s = {}
                if (this.isCssColor(event.bgcolor)) {
                    s['background-color'] = event.bgcolor
                    s.color = luminosity(event.bgcolor) > 0.5 ? 'black' : 'white'
                }
                if (timeStartPos) {
                    s.top = timeStartPos(event.time) + 'px'
                }
                if (timeDurationHeight) {
                    s.height = timeDurationHeight(event.duration) + 'px'
                }
                s['align-items'] = 'flex-start'
                return s
            },
            updateFormatters() {
                try {
                    this.dateFormatter = new Intl.DateTimeFormat(void 0, {
                        year: 'numeric',
                        month: '2-digit',
                        day: '2-digit',
                        hour: '2-digit',
                        minute: '2-digit',
                        second: '2-digit',
                        hour12: false,
                        timeZone: 'UTC'
                    })
                    this.titleFormatter = new Intl.DateTimeFormat(void 0, {
                        month: this.shortMonthLabel ? 'short' : 'long',
                        year: 'numeric',
                        timeZone: 'UTC'
                    })
                } catch (e) {
                    // console.error('Intl.DateTimeFormat not supported')
                    this.dateFormatter = void 0
                    this.titleFormatter = void 0
                }
            },
            getEvents(dt) {
                const currentDate = QCalendar.parseTimestamp(dt)
                const events = []
                for (let i = 0; i < this.events.length; ++i) {
                    let added = false
                    if (this.events[i].date === dt) {
                        if (this.events[i].time) {
                            if (events.length > 0) {
                                // check for overlapping times
                                const startTime = QCalendar.parseTimestamp(this.events[i].date + ' ' + this.events[i].time)
                                const endTime = QCalendar.addToDate(startTime, {minute: this.events[i].duration})
                                for (let j = 0; j < events.length; ++j) {
                                    const startTime2 = QCalendar.parseTimestamp(events[j].date + ' ' + events[j].time)
                                    const endTime2 = QCalendar.addToDate(startTime2, {minute: events[j].duration})
                                    if (QCalendar.isBetweenDates(startTime, startTime2, endTime2) || QCalendar.isBetweenDates(endTime, startTime2, endTime2)) {
                                        events[j].side = 'left'
                                        this.events[i].side = 'right'
                                        events.push(this.events[i])
                                        added = true
                                        break
                                    }
                                }
                            }
                        }
                        if (!added) {
                            this.events[i].side = void 0
                            events.push(this.events[i])
                        }
                    } else if (this.events[i].days) {
                        // check for overlapping dates
                        const startDate = QCalendar.parseTimestamp(this.events[i].date)
                        const endDate = QCalendar.addToDate(startDate, {day: this.events[i].days})
                        if (QCalendar.isBetweenDates(currentDate, startDate, endDate)) {
                            events.push(this.events[i])
                            added = true
                        }
                    }
                }
                return events
            }
        }
    }
</script>
<style>

</style>
