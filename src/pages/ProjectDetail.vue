<template>
  <main class="project-detail-page">
    <button class="project-back" type="button" @click="goBack">
      <span class="project-back-icon">←</span>
      <span>Back</span>
    </button>

    <section class="project-detail-wrapper">
      <header class="project-header">
        <span class="project-status">Project</span>
        <h1 class="project-title">{{ project.title }}</h1>
        <p class="project-description">{{ project.description }}</p>
      </header>

      <div class="project-content">
        <article class="project-section">
          <h2>Overview</h2>
          <p v-for="(point, i) in project.overview" :key="i">{{ point }}</p>
        </article>

        <article class="project-section">
          <h2>Core Idea</h2>
          <p v-for="(point, i) in project.coreIdea" :key="i">{{ point }}</p>
        </article>

        <article class="project-section">
          <h2>Key Features</h2>
          <ul class="feature-list">
            <li v-for="(feature, i) in project.features" :key="i">
              {{ feature }}
            </li>
          </ul>
        </article>

        <article class="project-section">
          <h2>Tech Stack</h2>
          <div class="tech-tags">
            <span v-for="(tech, i) in project.tech" :key="i" class="tech-tag">{{
              tech
            }}</span>
          </div>
        </article>

        <article
          class="project-section project-links"
          v-if="project.github || project.demo"
        >
          <a
            v-if="project.github"
            :href="project.github"
            target="_blank"
            class="link-btn"
          >
            GitHub →
          </a>
          <a
            v-if="project.demo"
            :href="project.demo"
            target="_blank"
            class="link-btn demo"
          >
            Live Demo →
          </a>
        </article>
      </div>
    </section>
  </main>
  <Footer />
</template>

<script setup>
import { computed, inject } from "vue";
import { useRouter, useRoute } from "vue-router";
import Footer from "@/components/Footer.vue";

const router = useRouter();
const route = useRoute();
const startPageTransition = inject("startPageTransition", null);

// ✅ ALL YOUR PROJECT DATA HERE
const projectData = {
  music: {
    title: "Music Scheduler App",
    description: "A platform to schedule and enjoy music seamlessly.",
    overview: [
      "Music Scheduler lets users automate song playback at specific times.",
      "Users can build playlists, like songs, and manage their library.",
      "Real-time updates keep playback in sync across sessions.",
    ],
    coreIdea: [
      "Automate music playback so users don't have to manually play songs.",
      "Give full control over listening experience through scheduling.",
    ],
    features: [
      "Schedule songs to play at specific times",
      "Create and manage playlists",
      "Like and save favorite tracks",
      "Real-time playback updates",
      "Admin panel for management",
    ],
    tech: ["Node.js", "MongoDB", "Socket.io", "JWT", "Cloudinary"],
    github: "https://github.com/rakshand11/music-mern",
    demo: "https://music-mern-wx89.vercel.app",
  },

  restaurant: {
    title: "Restaurant Management System",
    description: "Smart dining and ordering experience for users and admins.",
    overview: [
      "Users can book tables in advance and browse the full menu.",
      "Food orders are placed online with real-time status updates.",
      "Admins get a full dashboard to manage orders, users, and menu items.",
    ],
    coreIdea: [
      "Simplify restaurant operations through a unified platform.",
      "Improve the customer experience from table booking to payment.",
    ],
    features: [
      "Table booking system",
      "Online food ordering and menu browsing",
      "Admin dashboard for full control",
      "Automatic discount on orders above ₹1000",
    ],
    tech: ["Node.js", "MongoDB", "Express", "JWT", "Cloudinary"],
    github: "https://github.com/rakshand11/restaurant-mern",
    demo: "https://restaurant-mern-sable.vercel.app/",
  },

  flatmate: {
    title: "Flatmate Finder",
    description: "Find compatible flatmates with secure profile matching.",
    overview: [
      "Flatmate Finder helps users discover compatible flatmates in their city.",
      "Users set up profiles with bio, photos, preferences, and location.",
      "Chat is only unlocked after both users match — keeping it safe.",
    ],
    coreIdea: [
      "Make finding flatmates easy, relevant, and secure.",
      "Prevent random messaging by requiring a match before chatting.",
    ],
    features: [
      "Filter by city, locality, age, gender, and lifestyle",
      "Profile matching with bio and photo support",
      "Chat only after successful match",
    ],
    tech: [
      "TypeScript",
      "Node.js",
      "PostgreSQL (Neon)",
      "Express",
      "Socket.io",
    ],
    github: "https://github.com/rakshand11/flatmate-finder",
    demo: "https://flatmate.rakshand.site",
  },
};

// ✅ Read project ID from URL query
const projectId = route.query.project;
const project = computed(() => projectData[projectId] || projectData["music"]);

const goBack = () => {
  const navigate = () => {
    router.push({ name: "home", query: { section: "projects-anchor" } });
  };
  if (startPageTransition) {
    startPageTransition(navigate);
  } else {
    navigate();
  }
};
</script>

<style scoped>
.project-detail-page {
  min-height: 100vh;
  padding: clamp(6rem, 10vw, 8rem) clamp(1.25rem, 5vw, 4.5rem)
    clamp(3rem, 8vw, 6rem);
  color: var(--theme-text-strong);
  position: relative;
}

.project-detail-wrapper {
  max-width: 720px;
  margin: 0 auto;
  display: flex;
  flex-direction: column;
  gap: 1.5rem;
}

.project-back {
  position: absolute;
  top: 1.25rem;
  left: clamp(1.5rem, 5vw, 4rem);
  z-index: 101;
  display: inline-flex;
  align-items: center;
  gap: 0.6rem;
  font-size: 0.95rem;
  font-weight: 600;
  letter-spacing: 0.08em;
  text-transform: uppercase;
  color: var(--theme-text-muted);
  background: transparent;
  border: none;
  cursor: pointer;
  transition:
    color 0.2s ease,
    transform 0.2s ease;
}

.project-back:hover {
  color: var(--theme-text-strong);
  transform: translateX(-4px);
}

.project-header {
  display: flex;
  flex-direction: column;
  gap: 0.75rem;
}

.project-status {
  display: inline-block;
  width: fit-content;
  padding: 0.4rem 1rem;
  font-size: 0.875rem;
  font-weight: 600;
  letter-spacing: 0.1em;
  text-transform: uppercase;
  color: var(--theme-cta-text);
  background: var(--theme-cta-bg);
  border-radius: 2rem;
}

.project-title {
  font-size: clamp(2rem, 5vw, 3rem);
  font-weight: 700;
  line-height: 1.15;
  letter-spacing: -0.02em;
  margin: 0;
}

.project-description {
  font-size: 1.1rem;
  color: var(--theme-text-muted);
  margin: 0;
}

.project-content {
  display: flex;
  flex-direction: column;
  gap: 1.75rem;
}

.project-section h2 {
  font-size: 1.35rem;
  font-weight: 600;
  margin: 0 0 0.5rem 0;
  color: var(--theme-text-strong);
}

.project-section p {
  margin: 0 0 0.75rem 0;
  font-size: 1.05rem;
  line-height: 1.7;
  color: var(--theme-text-muted);
}

/* Features list */
.feature-list {
  padding-left: 1.25rem;
  display: flex;
  flex-direction: column;
  gap: 0.4rem;
}

.feature-list li {
  font-size: 1.05rem;
  line-height: 1.7;
  color: var(--theme-text-muted);
}

/* Tech tags */
.tech-tags {
  display: flex;
  flex-wrap: wrap;
  gap: 0.5rem;
  margin-top: 0.25rem;
}

.tech-tag {
  padding: 0.3rem 0.85rem;
  font-size: 0.85rem;
  font-weight: 500;
  border: 1px solid var(--theme-text-muted);
  border-radius: 2rem;
  color: var(--theme-text-muted);
}

/* Links */
.project-links {
  display: flex;
  gap: 1rem;
  flex-wrap: wrap;
}

.link-btn {
  padding: 0.6rem 1.4rem;
  font-size: 0.95rem;
  font-weight: 600;
  border: 1px solid var(--theme-text-strong);
  border-radius: 2rem;
  color: var(--theme-text-strong);
  text-decoration: none;
  transition:
    background 0.2s ease,
    color 0.2s ease;
}

.link-btn:hover {
  background: var(--theme-text-strong);
  color: var(--theme-cta-text, #fff);
}

.link-btn.demo {
  background: var(--theme-cta-bg);
  color: var(--theme-cta-text);
  border-color: transparent;
}

@media (max-width: 768px) {
  .project-detail-page {
    padding-top: 5rem;
  }
  .project-section p,
  .feature-list li {
    font-size: 1rem;
  }
}
</style>
