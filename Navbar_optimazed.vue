<template>
  <div id="navbar" class="navbar">
    <div class="navbar__left_section">
      <NuxtLink :to="{ name: 'index' }" class="navbar__logo" />
      <div class="navbar__center_half ml_2">
        <NuxtLink
          v-for="(item, key) in navItems"
          :key="key"
          :to="toLinkQuestions(item.urlName)"
        >
          <div class="dropdown__item">
            <div class="dropdown__item_name">
              {{ item.name }}
            </div>
          </div>
        </NuxtLink>
      </div>
    </div>
    <div v-if="$auth.loggedIn" class="btn-resume">
      <button
        class="btn program-index__btn_start pl-3 pr-3"
        @click.prevent="toHistory"
      >
        Продолжить учебу
      </button>
    </div>
    <div class="navbar__right_section">
      <div class="navbar__center_half">
        <NuxtLink :to="{ name: 'favorites' }">
          <div class="dropdown__item">
            <div class="dropdown__icon dropdown_icon_favorites mr_1">
              <div v-show="favorites" class="dropdown__favorites_count">
                {{ favorites }}
              </div>
            </div>
            <div class="dropdown__item_name">
              Избранное
            </div>
          </div>
        </NuxtLink>

        <NuxtLink v-if="$auth.loggedIn" :to="{ name: 'profile' }">
          <div class="dropdown__item">
            <div class="dropdown__icon dropdown_icon_profile mr_1" />
            <div class="dropdown__item_name dropdown_item_name_profile">
              {{
                `${
                  $store.state.auth.user.surname
                    ? $store.state.auth.user.surname
                    : ""
                } ${$store.state.auth.user.name}`
              }}
            </div>
          </div>
        </NuxtLink>
      </div>
      <div @click="logout">
        <div class="dropdown__item dropdown_item-non-padding-right">
          <div class="dropdown__icon dropdown_icon_logout mr_1" />
          <div v-if="$auth.loggedIn" class="dropdown__item_name">
            Выход
          </div>
          <div v-else class="dropdown__item_name">
            Вход
          </div>
        </div>
      </div>
    </div>

    <div class="navbar__mobile">
      <div class="navbar_mobile_inner">
        <div
          class="navbar_logo_container"
          :class="[$route.name === 'index' ? '' : 'topbar_back_btn_indent']"
        >
          <div v-if="$route.name !== 'index'" class="topbar_inner">
            <a class="btn btn-back btn-non-focus" @click="backRoute">
              <span class="material-icons-outlined">
                arrow_back_ios
              </span>
            </a>
          </div>

          <NuxtLink :to="{ name: 'index' }" class="navbar__logo" />
        </div>

        <div class="navbar__control">
          <div class="navbar__search mr_4">
            <button
              v-if="$route.name === 'search'"
              class="btn navbar__search_btn non-focus"
              @click="openFilterSearch"
            >
              <svg height="22" viewBox="0 0 512 512" width="22">
                <path
                  d="m420.404 0h-328.808c-50.506 0-91.596 41.09-91.596 91.596v328.809c0 50.505 41.09 91.595 91.596 91.595h328.809c50.505 0 91.595-41.09 91.595-91.596v-328.808c0-50.506-41.09-91.596-91.596-91.596zm61.596 420.404c0 33.964-27.632 61.596-61.596 61.596h-328.808c-33.964 0-61.596-27.632-61.596-61.596v-328.808c0-33.964 27.632-61.596 61.596-61.596h328.809c33.963 0 61.595 27.632 61.595 61.596z"
                />
                <path
                  d="m432.733 112.467h-228.461c-6.281-18.655-23.926-32.133-44.672-32.133s-38.391 13.478-44.672 32.133h-35.661c-8.284 0-15 6.716-15 15s6.716 15 15 15h35.662c6.281 18.655 23.926 32.133 44.672 32.133s38.391-13.478 44.672-32.133h228.461c8.284 0 15-6.716 15-15s-6.716-15-15.001-15zm-273.133 32.133c-9.447 0-17.133-7.686-17.133-17.133s7.686-17.133 17.133-17.133 17.133 7.686 17.133 17.133-7.686 17.133-17.133 17.133z"
                />
                <path
                  d="m432.733 241h-35.662c-6.281-18.655-23.927-32.133-44.672-32.133s-38.39 13.478-44.671 32.133h-228.461c-8.284 0-15 6.716-15 15s6.716 15 15 15h228.461c6.281 18.655 23.927 32.133 44.672 32.133s38.391-13.478 44.672-32.133h35.662c8.284 0 15-6.716 15-15s-6.716-15-15.001-15zm-80.333 32.133c-9.447 0-17.133-7.686-17.133-17.133s7.686-17.133 17.133-17.133 17.133 7.686 17.133 17.133-7.686 17.133-17.133 17.133z"
                />
                <path
                  d="m432.733 369.533h-164.194c-6.281-18.655-23.926-32.133-44.672-32.133s-38.391 13.478-44.672 32.133h-99.928c-8.284 0-15 6.716-15 15s6.716 15 15 15h99.928c6.281 18.655 23.926 32.133 44.672 32.133s38.391-13.478 44.672-32.133h164.195c8.284 0 15-6.716 15-15s-6.716-15-15.001-15zm-208.866 32.134c-9.447 0-17.133-7.686-17.133-17.133s7.686-17.133 17.133-17.133 17.133 7.685 17.133 17.132-7.686 17.134-17.133 17.134z"
                />
              </svg>
            </button>
            <button
              class="btn navbar__search_btn non-focus"
              @click="showSearch = !showSearch"
            >
              <svg
                v-if="!showSearch"
                width="22"
                height="22"
                viewBox="0 0 22 22"
                fill="none"
              >
                <path
                  d="M21.6562 20.5517L15.7562 14.6517C17.174 12.9496 17.8811 10.7664 17.7302 8.55628C17.5793 6.34618 16.5822 4.27932 14.9463 2.78566C13.3103 1.29201 11.1615 0.48657 8.94683 0.536892C6.73215 0.587213 4.62213 1.48942 3.05572 3.05584C1.4893 4.62226 0.587091 6.73227 0.536769 8.94695C0.486448 11.1616 1.29189 13.3104 2.78554 14.9464C4.27919 16.5823 6.34606 17.5795 8.55616 17.7303C10.7663 17.8812 12.9494 17.1742 14.6515 15.7564L20.5515 21.6564L21.6562 20.5517ZM2.12498 9.15636C2.12498 7.76571 2.53736 6.40629 3.30996 5.25C4.08257 4.09372 5.1807 3.19251 6.46549 2.66033C7.75028 2.12815 9.16403 1.98891 10.528 2.26021C11.8919 2.53151 13.1447 3.20117 14.1281 4.18451C15.1114 5.16785 15.7811 6.4207 16.0524 7.78463C16.3237 9.14856 16.1844 10.5623 15.6523 11.8471C15.1201 13.1319 14.2189 14.23 13.0626 15.0026C11.9063 15.7752 10.5469 16.1876 9.15623 16.1876C7.29206 16.1855 5.50484 15.4441 4.18667 14.1259C2.86851 12.8077 2.12705 11.0205 2.12498 9.15636Z"
                  fill="black"
                />
              </svg>
              <svg
                v-else
                width="14"
                height="14"
                viewBox="0 0 14 14"
                fill="none"
              >
                <path
                  d="M12.7305 14L7.0045 8.26864L1.27846 14L0 12.7224L5.73505 7L0 1.27763L1.27846 0L7.0045 5.73136L12.7305 0.00899763L14 1.27763L8.27395 7L14 12.7224L12.7305 14Z"
                  fill="black"
                />
              </svg>
            </button>
          </div>
          <div class="navbar__burger">
            <div
              class="navbar__burger_icon"
              :class="{ navbar_open_menu: openMenu }"
              @click="openMenu = !openMenu"
            >
              <span class="navbar__burger_icon_line" />
              <span class="navbar__burger_icon_line" />
              <span class="navbar__burger_icon_line" />
              <span class="navbar__burger_icon_line" />
            </div>
          </div>
        </div>
      </div>
      <button class="btn btn-reload">
        <img
          :src="require('../../assets/images/programs/btn_reload_con.svg')"
          alt=""
          class="btn-reload__icon"
        />
      </button>
    </div>
    <transition name="menu">
      <searchable v-show="showSearch" class="searchable_sm" />
    </transition>
    <transition name="menu">
      <div v-show="openMenu" class="navbar__dropdown">
        <NuxtLink
          v-for="(item, key) in navItems"
          :key="key"
          :to="toLinkQuestions(item.urlName)"
          class="dropdown__item dropdown_item_border"
        >
          <div class="dropdown__icon dropdown_icon_page_label mr_1" />
          <div class="dropdown__item_name">
            {{ item.name }}
          </div>
        </NuxtLink>
        <div class="dropdown__item dropdown_item_border">
          <div class="dropdown__icon dropdown_icon_page_label mr_1" />
          <div class="dropdown__item_name">
            Вопросы и ответы
          </div>
        </div>
        <div class="dropdown__item dropdown_item_border">
          <div class="dropdown__icon dropdown_icon_page_label mr_1" />
          <div class="dropdown__item_name">
            Помощь
          </div>
        </div>
        <NuxtLink
          :to="{ name: 'favorites' }"
          class="dropdown__item dropdown_item_border"
        >
          <div class="dropdown__icon dropdown_icon_favorites mr_1">
            <div v-show="favorites" class="dropdown__favorites_count">
              {{ favorites }}
            </div>
          </div>
          <div class="dropdown__item_name">
            Избранное
          </div>
        </NuxtLink>
        <NuxtLink
          v-if="$auth.loggedIn"
          :to="{ name: 'profile' }"
          class="dropdown__item dropdown_item_border"
        >
          <div class="dropdown__icon dropdown_icon_profile mr_1" />
          <div class="dropdown__item_name">
            {{
              `${
                $store.state.auth.user.surname
                  ? $store.state.auth.user.surname
                  : ""
              } ${$store.state.auth.user.name}`
            }}
          </div>
        </NuxtLink>
        <div class="dropdown__item" @click="logout">
          <div class="dropdown__icon dropdown_icon_logout mr_1" />
          <div v-if="$auth.loggedIn" class="dropdown__item_name">
            Выход
          </div>
          <div v-else class="dropdown__item_name">
            Вход
          </div>
        </div>
        <div class="dropdown__item dropdown_item_border">
          <div class="mr_1" />
          <div class="dropdown__item_name">
            +7 999 99-88-77
          </div>
        </div>
        <div class="dropdown__item dropdown_item_border">
          <div class=" mr_1" />
          <div class="dropdown__item_name">
            <!-- Соцсети -->
            <div class="footers__social-links mr_2">
              <div class="footers__contact-item">
                <svg
                  height="16"
                  viewBox="-23 -21 682 682.66669"
                  width="16"
                  xmlns="http://www.w3.org/2000/svg"
                >
                  <path
                    d="m544.386719 93.007812c-59.875-59.945312-139.503907-92.9726558-224.335938-93.007812-174.804687 0-317.070312 142.261719-317.140625 317.113281-.023437 55.894531 14.578125 110.457031 42.332032 158.550781l-44.992188 164.335938 168.121094-44.101562c46.324218 25.269531 98.476562 38.585937 151.550781 38.601562h.132813c174.785156 0 317.066406-142.273438 317.132812-317.132812.035156-84.742188-32.921875-164.417969-92.800781-224.359376zm-224.335938 487.933594h-.109375c-47.296875-.019531-93.683594-12.730468-134.160156-36.742187l-9.621094-5.714844-99.765625 26.171875 26.628907-97.269531-6.269532-9.972657c-26.386718-41.96875-40.320312-90.476562-40.296875-140.28125.054688-145.332031 118.304688-263.570312 263.699219-263.570312 70.40625.023438 136.589844 27.476562 186.355469 77.300781s77.15625 116.050781 77.132812 186.484375c-.0625 145.34375-118.304687 263.59375-263.59375 263.59375zm144.585938-197.417968c-7.921875-3.96875-46.882813-23.132813-54.148438-25.78125-7.257812-2.644532-12.546875-3.960938-17.824219 3.96875-5.285156 7.929687-20.46875 25.78125-25.09375 31.066406-4.625 5.289062-9.242187 5.953125-17.167968 1.984375-7.925782-3.964844-33.457032-12.335938-63.726563-39.332031-23.554687-21.011719-39.457031-46.960938-44.082031-54.890626-4.617188-7.9375-.039062-11.8125 3.476562-16.171874 8.578126-10.652344 17.167969-21.820313 19.808594-27.105469 2.644532-5.289063 1.320313-9.917969-.664062-13.882813-1.976563-3.964844-17.824219-42.96875-24.425782-58.839844-6.4375-15.445312-12.964843-13.359374-17.832031-13.601562-4.617187-.230469-9.902343-.277344-15.1875-.277344-5.28125 0-13.867187 1.980469-21.132812 9.917969-7.261719 7.933594-27.730469 27.101563-27.730469 66.105469s28.394531 76.683594 32.355469 81.972656c3.960937 5.289062 55.878906 85.328125 135.367187 119.648438 18.90625 8.171874 33.664063 13.042968 45.175782 16.695312 18.984374 6.03125 36.253906 5.179688 49.910156 3.140625 15.226562-2.277344 46.878906-19.171875 53.488281-37.679687 6.601563-18.511719 6.601563-34.375 4.617187-37.683594-1.976562-3.304688-7.261718-5.285156-15.183593-9.253906zm0 0"
                    fill-rule="evenodd"
                  />
                </svg>
              </div>
              <div class="footers__contact-item">
                <svg
                  id="Bold"
                  height="16"
                  viewBox="0 0 24 24"
                  width="16"
                  xmlns="http://www.w3.org/2000/svg"
                >
                  <path
                    d="m9.417 15.181-.397 5.584c.568 0 .814-.244 1.109-.537l2.663-2.545 5.518 4.041c1.012.564 1.725.267 1.998-.931l3.622-16.972.001-.001c.321-1.496-.541-2.081-1.527-1.714l-21.29 8.151c-1.453.564-1.431 1.374-.247 1.741l5.443 1.693 12.643-7.911c.595-.394 1.136-.176.691.218z"
                  />
                </svg>
              </div>
              <div class="footers__contact-item">
                <svg
                  id="Bold"
                  height="16"
                  viewBox="0 0 24 24"
                  width="16"
                  xmlns="http://www.w3.org/2000/svg"
                >
                  <path
                    d="m23.155 13.893c.716-6.027-.344-9.832-2.256-11.553l.001-.001c-3.086-2.939-13.508-3.374-17.2.132-1.658 1.715-2.242 4.232-2.306 7.348-.064 3.117-.14 8.956 5.301 10.54h.005l-.005 2.419s-.037.98.589 1.177c.716.232 1.04-.223 3.267-2.883 3.724.323 6.584-.417 6.909-.525.752-.252 5.007-.815 5.695-6.654zm-12.237 5.477s-2.357 2.939-3.09 3.702c-.24.248-.503.225-.499-.267 0-.323.018-4.016.018-4.016-4.613-1.322-4.341-6.294-4.291-8.895.05-2.602.526-4.733 1.93-6.168 3.239-3.037 12.376-2.358 14.704-.17 2.846 2.523 1.833 9.651 1.839 9.894-.585 4.874-4.033 5.183-4.667 5.394-.271.09-2.786.737-5.944.526z"
                  />
                  <path
                    d="m12.222 4.297c-.385 0-.385.6 0 .605 2.987.023 5.447 2.105 5.474 5.924 0 .403.59.398.585-.005h-.001c-.032-4.115-2.718-6.501-6.058-6.524z"
                  />
                  <path
                    d="m16.151 10.193c-.009.398.58.417.585.014.049-2.269-1.35-4.138-3.979-4.335-.385-.028-.425.577-.041.605 2.28.173 3.481 1.729 3.435 3.716z"
                  />
                  <path
                    d="m15.521 12.774c-.494-.286-.997-.108-1.205.173l-.435.563c-.221.286-.634.248-.634.248-3.014-.797-3.82-3.951-3.82-3.951s-.037-.427.239-.656l.544-.45c.272-.216.444-.736.167-1.247-.74-1.337-1.237-1.798-1.49-2.152-.266-.333-.666-.408-1.082-.183h-.009c-.865.506-1.812 1.453-1.509 2.428.517 1.028 1.467 4.305 4.495 6.781 1.423 1.171 3.675 2.371 4.631 2.648l.009.014c.942.314 1.858-.67 2.347-1.561v-.007c.217-.431.145-.839-.172-1.106-.562-.548-1.41-1.153-2.076-1.542z"
                  />
                  <path
                    d="m13.169 8.104c.961.056 1.427.558 1.477 1.589.018.403.603.375.585-.028-.064-1.346-.766-2.096-2.03-2.166-.385-.023-.421.582-.032.605z"
                  />
                </svg>
              </div>
            </div>
            <!-- Соцсети -->
          </div>
        </div>
      </div>
    </transition>
  </div>
</template>

<script>
import Searchable from "~/components/programs/blocks/Searchable";
import { eventBus } from "~/components/bus";

export default {
  name: "navbar",
  components: { Searchable },
  data() {
    return {
      openMenu: false,
      showSearch: false,
      showFilter: false
    };
  },
  computed: {
    favorites() {
      return this.$store.getters["profile/countWish"];
    },
    navItems() {
      return this.$store.state.env.navItems;
    }
  },
  methods: {
    backRoute() {
      this.$router.back();
    },
    logout() {
      if (this.$auth.loggedIn) {
        this.$auth.$storage.setUniversal("_token_expiration.local", "false");
        this.$auth.$storage.setUniversal("_token.local", "false");
        this.$router.push({ name: "login" });
      } else {
        this.$router.push({
          name: "login"
        });
      }
    },
    openFilterSearch() {
      this.showFilter = !this.showFilter;
      eventBus.$emit("open-filter", this.showFilter);
    },
    async toHistory() {
      await this.$axios.get(`/student/history`).then(response => {
        this.toLink(response.data);
      });
    },
    toLink(history) {
      const isCourse = Array.isArray(history.education_program) ? true : false;
      if (history.item.type === "kim") {
        this.$router.push({
          name: isCourse
            ? "courses-course-kim"
            : "programs-program_id-course_id-test-kim_id",
          params: isCourse
            ? {
                course: history.course.slug,
                kim: history.item.slug
              }
            : {
                program_id: history.education_program.slug,
                course_id: history.course.slug,
                kim_id: history.item.slug
              }
        });
      } else {
        this.$router.push({
          name: isCourse
            ? "courses-course-lecture"
            : "programs-program_id-course_id-lecture_id",
          params: isCourse
            ? {
                course: history.course.slug,
                lecture: history.item.slug
              }
            : {
                program_id: history.education_program.slug,
                course_id: history.course.slug,
                lecture_id: history.item.slug
              }
        });
      }
    },
    toLinkQuestions(path) {
      const lengthParams = Object.keys(this.$route.params).length;
      const keysParams = Object.keys(this.$route.params)
      let name;
      let params = {};
      if (path !== "questions") {
        name = path;
      } else {
        if (lengthParams === 0) {
          name = path;
        }
        if (lengthParams === 1) {
          if (keysParams[0] === "category_id") {
            name = path;
          } else {
            name =
              keysParams[0] === "course"
                ? "questions-courses-course"
                : "questions-programs-program_slug";
            params =
              keysParams[0] === "course"
                ? {}
                : { program_slug: this.$route.params.program_id };
          }
        }
        if (lengthParams === 2) {
          if (
            keysParams[0] === "program_id" &&
            keysParams[1] === "course_id"
          ) {
            name = "questions-programs-program_slug-course_slug";
            params = {
              program_slug: this.$route.params.program_id,
              course_slug: this.$route.params.course_id
            };
          }
          if (keysParams[0] === "course") {
            name =
              keysParams[1] === "lecture"
                ? "questions-courses-course-lecture"
                : "questions-courses-course-kim";
          }
        }
        if (lengthParams === 3) {
            name = 
              keysParams[2] === "lecture_id"
                ? "questions-programs-program_slug-course_slug-lecture_slug"
                : "questions-programs-program_slug-course_slug-test-kim_id"
            params = 
              keysParams[2] === "lecture_id"
                ? {
                  program_slug: this.$route.params.program_id,
                  course_slug: this.$route.params.course_id,
                  lecture_slug: this.$route.params.lecture_id
                }
                : {
                  program_slug: this.$route.params.program_id,
                  course_slug: this.$route.params.course_id,
                  kim_id: this.$route.params.kim_id
                }
        }
      }
      return {
        name,
        params
      };
    }
  }
};
</script>

<style scoped>
.btn-resume {
  margin-left: auto;
  padding-right: 20px;
}
@media (max-width: 1060px) {
  .btn.btn-reload {
    display: none;
  }
}
@media (max-width: 1168px) {
  .btn-resume {
    display: none;
  }
}
</style>
