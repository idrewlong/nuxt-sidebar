<template>
  <div
    :class="['side-nav', { expanded: isExpanded }]"
    @mouseenter="expand"
    @mouseleave="collapse"
  >
    <div class="logo-container">
      <img src="/images/IDLOGO.png" alt="Logo" class="logo" />
    </div>
    <nav>
      <ul>
        <li v-for="item in menuItems" :key="item.name">
          <div
            class="menu-item"
            @click="toggleSubMenu(item)"
            :class="{ active: activeItem === item.name }"
          >
            <i :class="item.icon"></i>
            <span class="menu-text">{{ item.name }}</span>
            <i
              v-if="item.subItems"
              class="fas fa-chevron-down arrow"
              :class="{ rotated: item.expanded }"
            ></i>
          </div>
          <ul
            v-if="item.subItems"
            class="sub-menu"
            :class="{ show: item.expanded }"
          >
            <li v-for="subItem in item.subItems" :key="subItem.name">
              <a :href="subItem.link">{{ subItem.name }}</a>
            </li>
          </ul>
        </li>
      </ul>
    </nav>
  </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from "vue";

const isExpanded = ref(false);
const activeItem = ref("");
const isMobile = ref(false);

const menuItems = ref([
  { name: "Dashboard", icon: "fas fa-tachometer-alt", link: "/dashboard" },
  {
    name: "Products",
    icon: "fas fa-box",
    expanded: false,
    subItems: [
      { name: "All Products", link: "/products" },
      { name: "Add New", link: "/products/new" },
      { name: "Categories", link: "/products/categories" },
    ],
  },
  { name: "Orders", icon: "fas fa-shopping-cart", link: "/orders" },
  { name: "Customers", icon: "fas fa-users", link: "/customers" },
  { name: "Settings", icon: "fas fa-cog", link: "/settings" },
]);

const expand = () => {
  if (!isMobile.value) {
    isExpanded.value = true;
  }
};

const collapse = () => {
  if (!isMobile.value) {
    isExpanded.value = false;
  }
};

const toggleSubMenu = (item) => {
  if (item.subItems) {
    item.expanded = !item.expanded;
  }
  activeItem.value = item.name;
};

const checkMobile = () => {
  isMobile.value = window.innerWidth <= 768;
  if (isMobile.value) {
    isExpanded.value = false;
  }
};

onMounted(() => {
  checkMobile();
  window.addEventListener("resize", checkMobile);
});

onUnmounted(() => {
  window.removeEventListener("resize", checkMobile);
});
</script>

<style scoped>
.side-nav {
  position: fixed;
  top: 0;
  left: 0;
  height: 100vh;
  background-color: #2c3e50;
  color: white;
  width: 60px;
  transition: width 0.3s ease;
  overflow-x: hidden;
  z-index: 1000;
}

.side-nav.expanded {
  width: 240px;
}

.logo-container {
  padding: 20px 0;
  text-align: center;
}

.logo {
  width: 40px;
  height: 40px;
  transition: all 0.3s ease;
}

.expanded .logo {
  width: 120px;
  height: auto;
}

nav ul {
  list-style-type: none;
  padding: 0;
  margin: 0;
}

.menu-item {
  display: flex;
  align-items: center;
  padding: 15px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.menu-item:hover,
.menu-item.active {
  background-color: #34495e;
}

.menu-item i {
  font-size: 20px;
  width: 30px;
  text-align: center;
}

.menu-text {
  margin-left: 10px;
  display: none;
}

.expanded .menu-text {
  display: inline;
}

.arrow {
  margin-left: auto;
  transition: transform 0.3s ease;
}

.arrow.rotated {
  transform: rotate(180deg);
}

.sub-menu {
  max-height: 0;
  overflow: hidden;
  transition: max-height 0.3s ease;
  background-color: #34495e;
}

.sub-menu.show {
  max-height: 500px;
}

.sub-menu li a {
  display: block;
  padding: 10px 15px 10px 55px;
  color: #ecf0f1;
  text-decoration: none;
  transition: background-color 0.3s ease;
}

.sub-menu li a:hover {
  background-color: #2c3e50;
}

@media (max-width: 768px) {
  .side-nav {
    width: 100%;
    height: auto;
    position: relative;
  }

  .side-nav.expanded {
    width: 100%;
  }

  .menu-text {
    display: inline;
  }

  .logo-container {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 10px;
  }

  .logo {
    width: 40px;
    height: 40px;
  }

  .expanded .logo {
    width: 40px;
  }
}
</style>
