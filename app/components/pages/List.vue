<template>
  <main>
    <div class="container ombre">
      <header-component/>
      <div id="tips" class="row">

        <div id="sticky" class="hidden-xs cats" :style="scrollMenu.sticky">
          <div
            class="scroller scroller-left"
            v-if="scrollMenu.btnLeft"
            @click="scrollMenu.left += scrollMenu.visibleWidth; scrollMenuRefresh();">
            <i class="fa fa-chevron-left" aria-hidden="true"></i>
          </div>
          <div
            class="scroller scroller-right"
            v-if="scrollMenu.btnRight"
            @click="scrollMenu.left -= scrollMenu.visibleWidth; scrollMenuRefresh();">
            <i class="fa fa-chevron-right" aria-hidden="true"></i>
          </div>
          <nav class="navbar navbar-default nav-cats" role="navigation">
            <div class="collapse navbar-collapse" id="navbar-collapse-1">
              <ul
                class="nav navbar-nav nav-tabs" role="tablist"
                :style="'left: ' + scrollMenu.left + 'px'">
                <li>
                  <a href="#tagssearch" :title="$t('txt.searchByTags')">
                    <i class="fa fa-lg fa-search" aria-hidden="true"></i>
                    <span class="sr-only">{{ $t('txt.searchByTags') }}</span>
                  </a>
                </li>
                <li><a href="#all">{{ $t('txt.allServices') }}</a></li>
                <li v-for="(icon, cat) in $root.cat2.icons">
                  <a :href="'#' + cat" :title="$t('cat2.' + cat)">{{ $t('cat2.' + cat) }}</a></li>
              </ul>
            </div>
          </nav>
        </div>

        <a class="anchor" id="tagssearch" rel="nofollow"></a>
        <div class="clearfix" style="margin:30px auto">
          <div class="col-sm-6 col-sm-offset-3 well">
            <div class="clearfix" style="margin:10px 0">
              <label class="col-sm-1 text-right" for="tags-select">
                <i class="fa fa-2x fa-search"></i>
                <span class="sr-only">{{ $t('txt.searchByTags') }}</span>
              </label>
              <div class="col-sm-11">
                <v-select id="tags-select" multiple
                  :options="tags($t('services')).concat(tags($root.services, 'gafam'))"
                  :placeholder="$t('txt.searchByTags')"
                  v-model="results"
                ></v-select>
              </div>
            </div>
            <div class="clearfix h4">
              <ul class="list-inline text-center">
                <li v-for="(tag) in $t('tags')">
                  <a :href="'#tag-' + tag" class="btn btn-xs btn-default"
                    @click="(results.indexOf(tag) === -1) ? results.push(tag) : results.pop(tag) ;"
                  >{{ tag }}</a>
                </li>
              </ul>
            </div>
          </div>
        </div>

        <div class="panel-group col-xs-12">
          <div  id="results_wrapper" class="panel panel-default clearfix" v-show="results.length > 0">
            <div class="panel-heading">
              <h2 class="panel-title text-center h1">
                {{ $t('txt.results') }}
              </h2>
            </div>
            <div id="results" class="clearfix" >
              <article
                v-for="(service, key) in $root.services"
                v-if="($root.fight.indexOf(key) > -1)"
                v-show="isInResults($t('services.' + key + '.tags') + ', ' + $root.services[key].gafam[0], results)"
                class="col-md-3 col-sm-6 text-center">
                <h3>
                  <i :class="'fa fa-2x fa-' + service.i"></i><br>
                  <p v-html="service.F"></p>
                </h3>
                <p class="desc" v-html="$t('services.' + key + '.tDesc')"></p>
                <p><img class="img-responsive" :src="$root['/'] +'img/screens/' + noFrama(service.F) + '.png'" alt="" /></p>
                <div class="clearfix">
                  <a :href="service.FL" class="btn btn-link btn-lg pull-left text-uppercase">{{ $t('txt.use') }}</a>

                  <dropdown dropup ref="dropdown" menu-right class="pull-right">
                    <btn type="button" class="btn btn-link btn-lg dropdown-toggle"
                      aria-haspopup="true" aria-expanded="false" :id="'dropdown-' + key">
                      <i class="fa fa-lg fa-question-circle-o" aria-hidden="true"></i>
                      <span class="sr-only">{{ $t('txt.plus') }}</span>
                    </btn>
                    <template slot="dropdown">
                      <li><a href="javascript:void(0);" data-toggle="modal" :data-target="'#modal-t-' + key"
                        @click="modal.open = true; modal.key = key;"
                        >{{ $t('txt.more') }}</a></li>
                      <li><a :href="$t('link.docs') + text(service.S).toLowerCase()">{{ $t('txt.docs') }}</a></li>
                    </template>
                  </dropdown>

                </div>
              </article>
            </div>
          </div>
          <section v-for="(icon, cat) in $root.cat2.icons">
            <a class="anchor" :id="cat" rel="nofollow"></a>
            <div class="panel panel-default clearfix">
              <div class="panel-heading">
                <h2 class="panel-title text-center h1">
                  {{ $t('cat2.' + cat) }}
                </h2>
              </div>
              <div class="clearfix">
                <article
                  v-for="(service, key) in $root.services"
                  v-if="$root.fight.indexOf(key) > -1 && service.c2 === cat"
                  class="col-md-3 col-sm-6 text-center">
                  <h3>
                    <i :class="'fa fa-2x fa-' + service.i"></i><br>
                    <p v-html="service.F"></p>
                  </h3>
                  <p class="desc" v-html="$t('services.' + key + '.tDesc')"></p>
                  <p><img class="img-responsive" :src="$root['/'] +'img/screens/' + noFrama(service.F) + '.png'" alt="" /></p>
                  <div class="clearfix">
                    <a :href="service.FL" class="btn btn-link btn-lg pull-left text-uppercase">{{ $t('txt.use') }}</a>

                    <dropdown dropup ref="dropdown" menu-right class="pull-right">
                      <btn type="button" class="btn btn-link btn-lg dropdown-toggle"
                        aria-haspopup="true" aria-expanded="false" :id="'dropdown-' + key">
                        <i class="fa fa-lg fa-question-circle-o" aria-hidden="true"></i>
                        <span class="sr-only">{{ $t('txt.plus') }}</span>
                      </btn>
                      <template slot="dropdown">
                        <li><a href="javascript:void(0);" data-toggle="modal" :data-target="'#modal-t-' + key"
                          @click="modal.open = true; modal.key = key;"
                          >{{ $t('txt.more') }}</a></li>
                        <li><a :href="$t('link.docs') + text(service.S).toLowerCase()">{{ $t('txt.docs') }}</a></li>
                      </template>
                    </dropdown>

                  </div>
                </article>
              </div>
            </div>
          </section>

          <!-- <modal> --------------------------------------------- -->
          <modal
            id="FramaModal"
            v-model="modal.open"
            :ok-text="$t('txt.close')"
            tabindex="-1"
            role="dialog"
            aria-labelledby="FramaLabel"
            aria-hidden="true"
            size="lg"
          >
            <!-- modal-header -->
            <div slot="title">
              <img class="pull-left" :src="'https://framasoft.org/nav/img/icons/' + noFrama($root.services[modal.key].F) + '.png'">
              <span class="frama" v-html="$root.services[modal.key].F"></span><br>
              <span class="desc" v-html="$t('services.' + modal.key + '.lDesc')"></span>
            </div>

            <!-- web-screen -->
            <div class="web-browser">
              <div class="toolbar">
                <img :src="$root['/'] +'img/browser-left.png'" alt="" />
                <div class="search-bar"></div>
                <img :src="$root['/'] +'img/browser-right.png'" alt="" />
              </div>
              <img
                :src="$root['/'] +'img/screens/' + noFrama($root.services[modal.key].F) + '-full.png'"
                class="img-responsive" alt=""
              />
            </div>

            <!-- desc -->
            <p v-html="$t('services.' + modal.key + '.mBody.desc').replace(/@framaservice/g, $root.services[modal.key].F)"></p>

            <!-- video / desc -->
            <p v-if="$t('services.' + modal.key + '.mBody.more') !== 'services.' + modal.key + '.mBody.more'"
               v-html="$t('services.' + modal.key + '.mBody.more').replace(/@framaservice/g, $root.services[modal.key].F)"></p>

            <!-- features -->
            <div v-if="$t('services.' + modal.key + '.mBody.feat') !== 'services.' + modal.key + '.mBody.feat'">
              <h5 class="h3 violet">{{ $t('txt.features') }}</h5>
              <ul v-if="Array.isArray($t('services.' + modal.key + '.mBody.feat'))">
                <li
                  v-for="(item) in $t('services.' + modal.key + '.mBody.feat')"
                  v-html="item.replace(/@framaservice/g, $root.services[modal.key].F)"
                ></li>
              </ul>
              <p
                v-else
                v-html="$t('services.' + modal.key + '.mBody.feat').replace(/@framaservice/g, $root.services[modal.key].F)">
              </p>
            </div>

            <!-- modal-footer -->
            <div slot="footer">
              <!-- tags buttons -->
              <ul class="list-inline col-md-6 text-left">
                <li
                  v-for="(tag) in $t('services.' + modal.key +'.tags').split(', ')"
                  v-if="tag !== '' && tag !== ' '">
                  <a
                    :href="'#tag-' + tag.replace(' ', '-').toLowerCase()"
                    class="btn btn-xs btn-default"
                    @click="modal.open = false; (results.indexOf(tag) === -1) ? results.push(tag) : results.pop(tag) ;"
                    v-scroll-to="'#results_wrapper'">
                    {{ tag.replace(/ /, '').toLowerCase() }}
                  </a></li>
              </ul>
              <!-- docs / install buttons -->
              <div class="col-md-6 text-right">
                <a :href="$t('link.docs') + text($root.services[modal.key].S.toLowerCase())"
                  class="btn btn-lg btn-link text-uppercase">{{ $t('txt.docs') }}</a>
                <a :href="$root.services[modal.key].FL" class="btn btn-lg btn-link text-uppercase">{{ $t('txt.use') }}</a>
              </div>
            </div>
          </modal>
          <!-- </modal> -------------------------------------------- -->

          <a class="anchor" id="all" rel="nofollow"></a>
          <div class="panel panel-default">
            <div class="panel-heading">
              <h2 class="panel-title text-center">
                {{ $t('txt.allServices') }}
              </h2>
            </div>
            <div class="clearfix">
              <ul class="list-inline">
                <li class="col-xs-4 col-sm-3 col-md-2 text-center" style="padding:20px"
                  v-for="(service, key) in $root.services" v-if="($root.fight.indexOf(key) > -1)">
                  <a :href="service.FL" class="btn btn-default btn-block"
                    :title="$t('services.' + key + '.sDesc')"
                    rel="popover" data-placement="bottom"
                    :data-content="$t('services.' + key + '.lDesc')">
                    <i :class="'fa fa-3x fa-fw fa-' + service.i + ' fc_g7'"></i>
                    <br/>
                    <span v-html="service.F"></span>
                  </a>
                </li>
              </ul>
            </div>
          </div>
        </div>
      </div>
      <Signature />
    </div>
    <BackTop />
  </main>
</template>

<script>
import { Modal, Dropdown, Btn, ScrollSpy } from 'uiv';
import HeaderComponent from '../partials/Header.vue';
import Signature from '../partials/Signature.vue';
import BackTop from '../partials/BackTop.vue';
import { rmDiacritics, text, sanitize, noFrama } from '../../tools';

export default {
  name: 'List',
  components: {
    HeaderComponent,
    Modal, Dropdown, Btn,
    Signature,
    BackTop,
  },
  data() {
    const hash = window.location.hash;
    const tags = [];
    if (hash && hash.indexOf('#tag-') > -1) {
      tags.push(hash.replace('#tag-', ''));
    }
    return {
      modal: {
        open: false,
        key: 'bitly',
      },
      results: tags,
      scrollMenu: {
        left: 0,
        listWidth: 0,
        visibleWidth: 0,
        btnLeft: false,
        btnRight: true,
        sticky: 'position: relative;'
      },
    }
  },
  mounted() {
    this.scrollMenuRefresh();
    window.onresize = () => {
      this.scrollMenuRefresh();
      this.stickyCSS();
    };
    window.onscroll = () => {
      this.stickyCSS();
    };

    $(document).ready(() => {
      // Sticky (to replace by https://uiv.wxsm.space/scroll-spy/)
      // (works thanks to the framanav actually)
      $('body').attr({
        'data-spy': 'scroll',
        'data-target': '#sticky .navbar'
      });
    });
  },
  methods: {
    text(html) {
      return text(html)
    },
    sanitize(html) {
      return sanitize(rmDiacritics(text(html)))
    },
    noFrama(html) {
      return noFrama(this.sanitize(html))
    },
    tags(services, type) {
      let tags = '';
      if (type === 'gafam') {
        Object.keys(services).forEach((k) => {
          if (Array.isArray(services[k].gafam) && this.$root.fight.indexOf(k) > -1) {
            tags += `, ${services[k].gafam[0]}`;
          }
        });
      } else {
        Object.keys(services).forEach((k) => {
          if (typeof services[k].tags === 'string') {
            tags += `, ${services[k].tags}`;
          }
        });
      }
      return tags
        .replace(/^, /, '')
        .split(', ')
        .sort()
        .filter((v, i, a) => a.indexOf(v) === i);
    },
    isInResults(tags, results) {
      let seen = false;
      results.forEach((val) => {
        if (tags.indexOf(val) > -1) {
          seen = true;
        }
      });
      return seen;
    },
    scrollMenuRefresh() {
      this.scrollMenu.listWidth = document.querySelector('.nav-cats .nav-tabs').offsetWidth;
      this.scrollMenu.visibleWidth = document.querySelector('.nav-cats').offsetWidth;
      if (this.scrollMenu.visibleWidth - this.scrollMenu.left < this.scrollMenu.listWidth) {
        this.scrollMenu.btnRight = true;
      } else {
        this.scrollMenu.btnRight = false;
        this.scrollMenu.left = this.scrollMenu.visibleWidth - this.scrollMenu.listWidth;
      };
      if (this.scrollMenu.left < 0) {
        this.scrollMenu.btnLeft = true;
      } else {
        this.scrollMenu.btnLeft = false;
        this.scrollMenu.left = 0;
      }
    },
    stickyCSS() {
      this.scrollMenu.sticky = 'position: relative;';
      if (document.documentElement.scrollTop > 640) {
        this.scrollMenu.sticky = `position: fixed; width: ${document.querySelector('#tips').offsetWidth}px;`;
      }
    },
  },
}
</script>
