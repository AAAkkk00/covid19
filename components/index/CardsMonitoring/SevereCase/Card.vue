<template>
  <v-col cols="12" :md="isSingleCard || 6" class="DataCard SevereCaseCard">
    <client-only>
      <chart
        :title="$t('モニタリング項目(6)')"
        title-id="positive-status-severe-case"
        :info-titles="[$t('重症患者数')]"
        chart-id="time-bar-chart-positive-status-severe-case"
        :chart-data="graphData"
        :date="date"
        :unit="$t('人')"
        :url="'https://catalog.data.metro.tokyo.lg.jp/dataset/t000010d0000000090'"
        :day-period="isSingleCard ? 120 : 60"
        :is-single-card="isSingleCard"
      >
        <template #additionalDescription>
          <span>{{ $t('（注）') }}</span>
          <ul>
            <li>
              {{
                $t(
                  '入院患者数のうち、人工呼吸器管理（ECMOを含む）が必要な患者数を計上。'
                )
              }}
              <app-link
                to="https://www.bousai.metro.tokyo.lg.jp/_res/projects/default_project/_page_/001/011/435/7kai/202008207.pdf"
              >
                {{ $t('重症基準の考え方はこちら') }}
              </app-link>
            </li>
            <li>
              {{
                $t(
                  '上記の考え方で重症患者数の計上を開始した2020年4月27日から作成'
                )
              }}
            </li>
          </ul>
        </template>
      </chart>
    </client-only>
  </v-col>
</template>

<script>
import AppLink from '@/components/_shared/AppLink.vue'
import Chart from '@/components/index/CardsMonitoring/SevereCase/Chart.vue'
import Data from '@/data/positive_status.json'
import { convertDateToISO8601Format } from '@/utils/formatDate.ts'
import { isSingleCard } from '@/utils/urls'

export default {
  components: {
    Chart,
    AppLink,
  },
  data() {
    const { date } = Data
    const graphData = Data.data
      .filter((d) => new Date(d.date) > new Date('2020-04-26'))
      .filter((d) => !isNaN(d.severe_case))
      .map((d) => ({
        label: convertDateToISO8601Format(d.date),
        transition: d.severe_case,
      }))
    return {
      graphData,
      date,
    }
  },
  computed: {
    isSingleCard() {
      return isSingleCard(this.$route.path)
    },
  },
}
</script>
