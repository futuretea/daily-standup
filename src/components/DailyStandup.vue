<template>
  <div>
    <Survey :survey="survey" id="surveyContainer" />
    <div v-if="isSurveyCompleted">
      <textarea v-model="surveyResults" class="sd-input sd-comment" cols="50" rows="5" style="resize: both;"/>
      <div id="copy" class="sv-action">
        <div class="sv-action__content">
         <input type="button" value="Copy" title="Copy" class="sd-btn" @click="copyResults">
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import 'survey-vue/defaultV2.min.css';
import { Model } from 'survey-vue';
import { Survey } from 'survey-vue';

const surveyJson = {
  pages: [
    {
      elements: [{
        type: "html",
        html: "<h2>今天又是充满希望的一天</h2>"
      }]
    },
    {
      elements: [{
        name: "yesterday",
        title: "What you worked on yesterday?",
        description: "分享昨天预设的工作，哪些任务完成了，哪些任务没有完成以及原因或者问题是什么",
        type: "comment",
        isRequired: true
      }]
    },
    {
      elements: [
        {
          name: "today",
          title: "What you plan on working on today?",
          description: "分享今天预计要解决的问题, 只需要选重点的1-2个任务即可, 这些任务今天能够完成或者部分完成",
          type: "comment",
          isRequired: true
        }
      ],
    },
    {
      elements: [
        {
          name: "blockers",
          title: "Any Blockers?",
          description: "如果遇到了block的问题需要团队成员的帮助, 可以在会议上提出。也许并不一定会立即得到答案，但是可以提醒相关的同事，让他们知道你需要帮助",
          type: "comment",
          isRequired: false
        }
      ],
    }
  ],
  showQuestionNumbers: "off",
  pageNextText: "Next",
  pagePrevText: "Previous",
  completeText: "Finish",
  showPrevButton: true,
  firstPageIsStarted: true,
  startSurveyText: "Let's go!",
  completedHtml: "Thank you for your sync up!",
  showPreviewBeforeComplete: "showAnsweredQuestions"
};

export default {
  name: 'DailyStandup',
  components: {
    Survey
  },
  data() {
    const survey = new Model(surveyJson);
    survey.onComplete.add(this.displayResults);

    return {
      survey,
      surveyResults: "",
      isSurveyCompleted: false
    }
  },
  methods: {
    displayResults(sender) {
      this.surveyResults = JSON.stringify(sender.data, null, 4);
      this.isSurveyCompleted = true;
    },
    copyResults: function () {
        this.$copyText(this.surveyResults).then(function (e) {
          alert('Copied')
          console.log(e)
        }, function (e) {
          alert('Can not copy')
          console.log(e)
        })
      }
  },
}
</script>