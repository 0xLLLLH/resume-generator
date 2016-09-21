<template>
    <article id="resume">
        <header class="container resume-header">
            <figure class="qrcode" v-if="basicInfo.qrcode">
                <img width="100%" height="100%"
                     :src="basicInfo.qrcode"/>
            </figure>
            <section class="title">
                <h1>{{basicInfo.name}}</h1>
                <h2>{{basicInfo.title}}</h2>
            </section>
            <section class="contact-block">
                <div class="contact" v-if="basicInfo.email">
                    <i class="fa fa-envelope" aria-hidden="true">
                        <a :href="'mailto:' + basicInfo.email">{{basicInfo.email}}</a>
                    </i>
                </div>
                <div class="contact" v-if="basicInfo.github">
                    <i class="fa fa-github" aria-hidden="true">
                        <a :href="basicInfo.github">{{basicInfo.github}}</a>
                    </i>
                </div>
                <div class="contact" v-if="basicInfo.wechat">
                    <i class="fa fa-weixin" aria-hidden="true">
                        {{basicInfo.wechat}}
                    </i>
                </div>
                <div class="contact" v-if="basicInfo.blog">
                    <i class="fa fa-rss-square" aria-hidden="true">
                        <a :href="basicInfo.blog">{{basicInfo.blog}}</a>
                    </i>
                </div>
            </section>
        </header>
        <article class="container resume-body">
            <div class="left-part">
                <section class="education">
                    <div class="sec-title">
                        <div class="title-icon">
                            <i class="fa fa-book" aria-hidden="true"> </i>
                        </div>
                        <div class="title-text">
                            <div class="title-text-ch">教育经历</div>
                            <div class="title-text-en">Education</div>
                        </div>
                    </div>
                    <div class="sec-content">
                        <ul class="time-line">
                            <li v-for="edu in education"
                                class="time-line-item">
                                <div class="time-line-title">{{edu.time}}</div>
                                <div class="time-line-content">
                                    <dl>
                                        <dt class="education-school">{{edu.school}}</dt>
                                        <dd class="education-major">{{edu.major}}</dd>
                                        <dd v-for="honor in edu.honors"
                                            class="education-honor">
                                            <span v-html="honor">
                                            </span>
                                        </dd>
                                    </dl>
                                </div>
                            </li>
                        </ul>
                    </div>
                </section>
                <section class="language">
                    <div class="sec-title">
                        <div class="title-icon">
                            <i class="fa fa-comments" aria-hidden="true"> </i>
                        </div>
                        <div class="title-text">
                            <div class="title-text-ch">语言水平</div>
                            <div class="title-text-en">Education</div>
                        </div>
                    </div>
                    <div class="sec-content">
                        <div class="language-score">
                            <div v-for="score,idx in language.languageScore"
                                 class="score">
                                <span class="cert-name">
                                    {{score.type}}
                                </span>
                                <div class="progress-bar">
                                    <div class="progress"
                                         :style="{width:language.languageScore[idx].score*100/language.languageScore[idx].max+'%'}">
                                        {{score.score}}
                                    </div>
                                </div>
                            </div>
                        </div>
                        <div v-for="desc in language.descriptions"
                             class="language-description">
                            {{desc}}
                        </div>
                    </div>
                </section>
                <section class="experience">
                    <div class="sec-title">
                        <div class="title-icon">
                            <i class="fa fa-rocket" aria-hidden="true"> </i>
                        </div>
                        <div class="title-text">
                            <div class="title-text-ch">项目与工作经验</div>
                            <div class="title-text-en">Experience</div>
                        </div>
                    </div>
                    <div class="sec-content">
                        <ul class="time-line">
                            <li v-for="exp in experience"
                                class="time-line-item">
                                <div v-if="exp.type!='project'" class="time-line-title">
                                    {{exp.place}}
                                    <div class="experience-time">
                                        {{exp.time}}
                                    </div>
                                </div>
                                <div v-else class="time-line-title">
                                    {{exp.subtype}}
                                    <a v-show="exp.demo" class="link" :href="exp.demo" target="_blank">Demo</a>
                                    <a v-show="exp.link" class="link" :href="exp.link" target="_blank">源代码</a>
                                </div>
                                <div class="time-line-content">
                                    <dl>
                                        <dt v-show="exp.position" class="experience-position">{{exp.position}}</dt>
                                        <dt v-show="exp.name" class="experience-project-name">{{exp.name}}</dt>
                                        <dd v-for="desc in exp.descriptions"
                                            class="experience-description">
                                            <span v-html="desc">
                                            </span>
                                        </dd>
                                    </dl>
                                </div>
                            </li>
                        </ul>
                    </div>
                </section>
            </div>
            <div class="right-part">
                <section class="skills">
                    <div class="sec-title">
                        <div class="title-icon">
                            <i class="fa fa-wrench" aria-hidden="true"> </i>
                        </div>
                        <div class="title-text">
                            <div class="title-text-ch">技能</div>
                            <div class="title-text-en">Skills</div>
                        </div>
                    </div>
                    <div class="sec-content">
                        <ul class="time-line">
                            <li v-for="skill in skills"
                                class="time-line-item">
                                <div class="time-line-title">{{skill.typeName}}</div>
                                <div class="time-line-content">
                                    <dl>
                                       <template v-for="item in skill.detail">
                                           <dt class="skill-name">{{item.name}}</dt>
                                           <dd v-for="desc in item.descriptions"
                                            class="skill-description">
                                               <span v-html="desc">
                                               </span>
                                           </dd>
                                       </template>
                                    </dl>
                                </div>
                            </li>
                        </ul>
                    </div>
                </section>
            </div>
        </article>
    </article>
</template>

<script>

require('style!css!less!font-awesome-webpack/font-awesome-styles.loader!font-awesome-webpack/font-awesome.config.js');
require('style!css!normalize.css');
require('./assets/SCSS/style.scss');

import ResumeData from '../resume.json'

export default {
    data () {
        return {
            Data: ResumeData,
            basicInfo: ResumeData.basicInfo,
            education: ResumeData.education,
            language: ResumeData.language,
            experience: ResumeData.experience,
            skills: ResumeData.skills
        }
    }
}

    document.querySelector("title").innerText = ResumeData.resumeTitle;
</script>

<style>
</style>
