<template>
  <div class="body dashboard">
    <div class="content-wrapper">
      <Header />
      <section class="widgets">
        <div class="container">
          <div class="columns is-tablet is-centered">
            <div class="column is-5 is-12-mobile">
              <article class="tile is-child">
                <p class="title is-4 has-text-white">Create a HASHTAG token</p>
                <Mint />
              </article>
            </div>
            <div class="divider is-vertical is-hidden-mobile">OR</div>
            <div class="divider is-hidden-tablet">OR</div>
            <div class="column is-5 is-12-mobile">
              <article class="tile is-child">
                <p class="title is-4 has-text-white">Post a Url</p>
                <UrlSearch />
              </article>
            </div>
          </div>
        </div>
      </section>
      <section class="main">
        <div class="container">
          <div class="tile is-ancestor">
            <div class="tile is-horizontal">
              <div class="tile is-parent is-6 is-12-mobile is-vertical">
                <div class="tile is-child box">
                  <article class="is-white">
                    <a
                      @click="
                        cardModal({
                          type: 'faq',
                          content: '020-what-is-hashtag-token',
                        })
                      "
                      class="
                        mdi mdi-help-circle-outline mdi-24px
                        is-pulled-right
                        has-text-grey
                      "
                    />
                    <h2 class="title is-5">Newest hashtags</h2>
                    <b-table
                      :data="hashtags ? hashtags.slice(0, 10) : []"
                      focusable
                    >
                      <template slot="footer" v-if="!isCustom">
                        <div class="has-text-right">
                          <router-link :to="{ name: 'hashtags' }"
                            >Browse hashtags </router-link
                          >&nbsp;
                          <b-icon
                            icon="arrow-right"
                            type="is-dark"
                            size="is-small"
                          >
                          </b-icon>
                        </div>
                      </template>
                      <template slot-scope="props">
                        <b-table-column field="name" label="Hashtag">
                          <hashtag :value="props.row.displayHashtag"></hashtag>
                        </b-table-column>
                        <b-table-column field="timestamp" label="Created">
                          <timestamp-from
                            :value="props.row.timestamp"
                          ></timestamp-from>
                        </b-table-column>
                        <b-table-column
                          field="creator"
                          label="Creator"
                          :visible="$screen.desktop"
                        >
                          <eth-account
                            :value="props.row.creator"
                            route="creator-detail"
                          ></eth-account>
                        </b-table-column>
                        <b-table-column
                          field="publisher"
                          label="Publisher"
                          :visible="$screen.widescreen"
                        >
                          <eth-account
                            :value="props.row.publisher"
                            route="publisher-detail"
                          ></eth-account>
                        </b-table-column>
                      </template>
                    </b-table>
                  </article>
                </div>
              </div>
              <div class="tile is-parent is-6 is-12-mobile">
                <div class="tile is-child box">
                  <article class="is-white">
                    <a
                      @click="
                        cardModal({
                          type: 'faq',
                          content: '030-what-is-tagged-content',
                        })
                      "
                      class="
                        mdi mdi-help-circle-outline mdi-24px
                        is-pulled-right
                        has-text-grey
                      "
                    />
                    <h2 class="title is-5">Recently tagged content</h2>
                    <b-table :data="tags || []" focusable>
                      <template slot="footer" v-if="!isCustom">
                        <div class="has-text-right">
                          <router-link :to="{ name: 'nfts' }"
                            >Browse tagged assets </router-link
                          >&nbsp;
                          <b-icon
                            icon="arrow-right"
                            type="is-dark"
                            size="is-small"
                          >
                          </b-icon>
                        </div>
                      </template>
                      <template slot-scope="props">
                        <b-table-column field="nftId" centered>
                          <router-link
                            :to="{
                              name: 'nft-detail',
                              params: {
                                type: 'nft',
                                contract: props.row.nftContract,
                                id: props.row.nftId,
                              },
                            }"
                          >
                            <img
                              :src="props.row.nftImage"
                              :alt="props.row.nftName"
                              class="nft-thumb"
                            />
                          </router-link>
                        </b-table-column>
                        <b-table-column field="nftName" label="Asset Name">
                          <nft-link
                            type="nft"
                            :name="props.row.nftName"
                            :contract="props.row.nftContract"
                            :id="props.row.nftId"
                          ></nft-link>
                        </b-table-column>
                        <b-table-column
                          field="projectName"
                          label="Project"
                          :visible="$screen.widescreen"
                        >
                          {{ props.row.nftContractName }}
                        </b-table-column>
                        <b-table-column field="hashtagName" label="Hashtag">
                          <hashtag
                            :value="props.row.hashtagDisplayHashtag"
                          ></hashtag>
                        </b-table-column>
                      </template>
                    </b-table>
                  </article>
                </div>
              </div>
            </div>
          </div>
          <div class="tile is-ancestor">
            <div class="tile is-horizontal">
              <div class="tile is-parent is-6 is-12-mobile">
                <div class="tile is-child box">
                  <article class="is-white">
                    <a
                      @click="
                        cardModal({
                          type: 'faq',
                          content: '045-what-is-creator',
                        })
                      "
                      class="
                        mdi mdi-help-circle-outline mdi-24px
                        is-pulled-right
                        has-text-grey
                      "
                    />
                    <h2 class="title is-5">Top creators</h2>
                    <b-table :data="creators || []">
                      <template slot="footer" v-if="!isCustom">
                        <div class="has-text-right">
                          <router-link :to="{ name: 'creators' }"
                            >Browse creators </router-link
                          >&nbsp;
                          <b-icon
                            icon="arrow-right"
                            type="is-dark"
                            size="is-small"
                          >
                          </b-icon>
                        </div>
                      </template>
                      <template slot-scope="props" focusable>
                        <b-table-column field="id" label="Creator">
                          <eth-account
                            :value="props.row.id"
                            route="creator-detail"
                          ></eth-account>
                        </b-table-column>
                        <b-table-column
                          field="mintedCount"
                          label="Hashtags"
                          centered
                        >
                          {{ props.row.mintCount }}
                        </b-table-column>
                        <b-table-column
                          field="tagCount"
                          label="Tag count"
                          centered
                        >
                          {{ props.row.tagCount }}
                        </b-table-column>
                        <b-table-column
                          field="revenue"
                          label="Revenue"
                          centered
                        >
                          <eth-amount :value="props.row.tagFees"></eth-amount>
                        </b-table-column>
                      </template>
                    </b-table>
                  </article>
                </div>
              </div>
              <div class="tile is-parent is-6 is-12-mobile">
                <div class="tile is-child box">
                  <article class="is-white">
                    <a
                      @click="
                        cardModal({
                          type: 'faq',
                          content: '040-what-is-a-publisher',
                        })
                      "
                      class="
                        mdi mdi-help-circle-outline mdi-24px
                        is-pulled-right
                        has-text-grey
                      "
                    />
                    <h2 class="title is-5">Top publishers</h2>
                    <b-table :data="publishers || []">
                      <template slot="footer" v-if="!isCustom">
                        <div class="has-text-right">
                          <router-link :to="{ name: 'publishers' }"
                            >Browse publishers </router-link
                          >&nbsp;
                          <b-icon
                            icon="arrow-right"
                            type="is-dark"
                            size="is-small"
                          >
                          </b-icon>
                        </div>
                      </template>
                      <template slot-scope="props" focusable>
                        <b-table-column field="id" label="Publisher">
                          <eth-account
                            :value="props.row.id"
                            route="publisher-detail"
                          ></eth-account>
                        </b-table-column>
                        <b-table-column
                          field="mintedCount"
                          label="Hashtags"
                          centered
                        >
                          {{ props.row.mintCount }}
                        </b-table-column>
                        <b-table-column
                          field="tagCount"
                          label="Tag count"
                          centered
                        >
                          {{ props.row.tagCount }}
                        </b-table-column>
                        <b-table-column
                          field="revenue"
                          label="Revenue"
                          centered
                        >
                          <eth-amount :value="props.row.tagFees"></eth-amount>
                        </b-table-column>
                      </template>
                    </b-table>
                  </article>
                </div>
              </div>
            </div>
          </div>
          <div class="tile is-ancestor">
            <div class="tile is-horizontal">
              <div class="tile is-parent is-6 is-12-mobile">
                <div class="tile is-child box">
                  <a
                    @click="
                      cardModal({
                        type: 'faq',
                        content: '060-what-is-a-tagger',
                      })
                    "
                    class="
                      mdi mdi-help-circle-outline mdi-24px
                      is-pulled-right
                      has-text-grey
                    "
                  />
                  <article class="is-white">
                    <h2 class="title is-5">Top taggers</h2>
                    <b-table :data="taggers || []" focusable>
                      <template slot="footer" v-if="!isCustom">
                        <div class="has-text-right">
                          <router-link :to="{ name: 'taggers' }"
                            >Browse taggers </router-link
                          >&nbsp;
                          <b-icon
                            icon="arrow-right"
                            type="is-dark"
                            size="is-small"
                          >
                          </b-icon>
                        </div>
                      </template>
                      <template slot-scope="props">
                        <b-table-column field="id" label="Tagger">
                          <eth-account
                            :value="props.row.id"
                            route="tagger-detail"
                          ></eth-account>
                        </b-table-column>
                        <b-table-column
                          field="tagCount"
                          label="Tag count"
                          centered
                        >
                          {{ props.row.tagCount }}
                        </b-table-column>
                      </template>
                    </b-table>
                  </article>
                </div>
              </div>
              <div class="tile is-parent is-6 is-12-mobile">
                <div class="tile is-child box">
                  <article class="is-white">
                    <a
                      @click="
                        cardModal({
                          type: 'faq',
                          content: '020-what-is-hashtag-token',
                        })
                      "
                      class="
                        mdi mdi-help-circle-outline mdi-24px
                        is-pulled-right
                        has-text-grey
                      "
                    />
                    <h2 class="title is-5">Popular hashtags</h2>
                    <b-table :data="popular || []" focusable>
                      <template slot="footer" v-if="!isCustom">
                        <div class="has-text-right">
                          <router-link :to="{ name: 'hashtags' }"
                            >Browse hashtags </router-link
                          >&nbsp;
                          <b-icon
                            icon="arrow-right"
                            type="is-dark"
                            size="is-small"
                          >
                          </b-icon>
                        </div>
                      </template>
                      <template slot-scope="props">
                        <b-table-column field="name" label="Hashtag">
                          <hashtag :value="props.row.displayHashtag"></hashtag>
                        </b-table-column>
                        <b-table-column
                          field="tagCount"
                          label="Tag count"
                          centered
                        >
                          {{ props.row.tagCount }}
                        </b-table-column>
                      </template>
                    </b-table>
                  </article>
                </div>
              </div>
            </div>
          </div>
          <div class="tile is-ancestor">
            <div class="tile is-horizontal">
              <div class="tile is-parent is-6 is-12-mobile">
                <div class="tile is-child box">
                  <a
                    @click="
                      cardModal({
                        type: 'faq',
                        content: '050-what-is-an-owner',
                      })
                    "
                    class="
                      mdi mdi-help-circle-outline mdi-24px
                      is-pulled-right
                      has-text-grey
                    "
                  />
                  <article class="is-white coming-soon">
                    <h2 class="title is-5">Top owners</h2>
                    <div class="coming-soon-img">
                      <a href="/auction"
                        ><img src="../assets/coming-soon-banner.png"
                      /></a>
                    </div>
                    <pseudo-owners />
                  </article>
                </div>
              </div>
            </div>
          </div>
        </div>
      </section>
    </div>
    <Footer />
  </div>
</template>

<script>
import EthAccount from "../components/EthAccount";
import EthAmount from "../components/EthAmount";
import Footer from "../components/Footer";
import Hashtag from "../components/Hashtag";
import Header from "../components/Header";
import MarkdownModal from "../components/MarkdownModal";
import Mint from "../components/Mint";
import NftLink from "../components/NftLink";
import PseudoOwners from "../components/PseudoOwners";
import UrlSearch from "../components/UrlSearch";

import { SNAPSHOT, FIRST_THOUSAND_HASHTAGS } from "@/queries";
//import { mapGetters } from "vuex";
import TimestampFrom from "../components/TimestampFrom";
//import HashtagValidationService from "@/services/HashtagValidationService";
//import debounce from "lodash/debounce";

export default {
  name: "Hashtags",
  components: {
    EthAccount,
    EthAmount,
    Footer,
    Hashtag,
    Header,
    Mint,
    NftLink,
    PseudoOwners,
    TimestampFrom,
    UrlSearch,
  },
  data() {
    return {
      isCustom: false,
    };
  },
  methods: {
    cardModal(props) {
      this.$buefy.modal.open({
        parent: this,
        component: MarkdownModal,
        props: props,
        hasModalCard: true,
        trapFocus: true,
      });
    },
  },

  apollo: {
    hashtags: {
      query: FIRST_THOUSAND_HASHTAGS,
      pollInterval: 1000, // ms
    },
    publishers: {
      query: SNAPSHOT,
      pollInterval: 1000, // ms
    },
    creators: {
      query: SNAPSHOT,
      pollInterval: 1000,
    },
    owners: {
      query: SNAPSHOT,
      pollInterval: 1000, // ms
    },
    tags: {
      query: SNAPSHOT,
      pollInterval: 1000, // ms
    },
    popular: {
      query: SNAPSHOT,
      pollInterval: 1000, // ms
    },
    platform: {
      query: SNAPSHOT,
      pollInterval: 1000, // ms
    },
    taggers: {
      query: SNAPSHOT,
      pollInterval: 1000, // ms
    },
  },
};
</script>

<style lang="scss" scoped>
section.widgets {
  padding-bottom: 5rem;
}

.modal-tag {
  padding: 1rem;
}
</style>
