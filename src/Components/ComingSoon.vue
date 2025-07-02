<template>
    <main class="page">
        <!-- ==== Hero / Banner ==== -->
        <section class="hero">
            <h1 class="title">✨ Rough N Tough is coming soon</h1>
            <p class="subtitle">
                We're polishing the experience. Leave us a message and we'll reach out the moment we
                launch!
            </p>

            <!-- Countdown (optional) -->
            <Countdown :target="launchDate" />
        </section>

        <!-- ==== Contact Form ==== -->
        <section class="contact">
            <h2 class="section-title">Contact us</h2>

            <form class="form" @submit.prevent="handleSubmit">
                <div class="field">
                    <label>Name</label>
                    <input v-model.trim="form.name" required placeholder="Enter name" />
                </div>

                <div class="field">
                    <label>E‑mail</label>
                    <input type="email" v-model.trim="form.email" required placeholder="Enter e-mail" />
                </div>

                <div class="field">
                    <label>Mobile No.</label>
                    <input type="tel" v-model.trim="form.mobile" pattern="^[0-9]{10}$"
                        placeholder="Enter 10-digit number" required />
                </div>

                <div class="field">
                    <label>Message</label>
                    <textarea v-model.trim="form.message" rows="4" required placeholder="Write a message here"/>
                </div>

                <button class="btn" :disabled="sending">
                    {{ sending ? 'Sending…' : 'Submit' }}
                </button>
            </form>

            <p v-if="status === 'ok'" class="success">✅ Thanks! We'll reply shortly.</p>
            <p v-if="status === 'err'" class="error">❌ Something went wrong. Please try again later.</p>
        </section>
        <footer>
            © 2025 RoughNTough. All rights reserved.
        </footer>
    </main>

</template>

<script setup>
/* ----------------------------------------------
   imports & reactive state
---------------------------------------------- */
import { ref } from 'vue'
import emailjs from '@emailjs/browser'
import Countdown from './Countdown.vue'

/* target date for the countdown (optional) */
const launchDate = new Date('2025-07-15T00:00:00+05:30') // 1 Oct 2025, IST

/* contact‑form data */
const form = ref({
    name: '',
    email: '',
    mobile: '',
    message: ''
})

const sending = ref(false)
const status = ref('') // '', 'ok', 'err'

/* ----------------------------------------------
   submit handler
---------------------------------------------- */
async function handleSubmit() {
    sending.value = true
    status.value = ''

    try {
        /* use ENV vars so the keys never hit Git */
        const serviceId = "service_2hfx0rb"
        const templateId = "template_h20gkrr"
        const publicKey = "eGK_TVffoDN2b4fyu"

        await emailjs.send(
            serviceId,
            templateId,
            {
                from_name: form.value.name,
                from_email: form.value.email,
                from_mobile: form.value.mobile,
                message: form.value.message
            },
            publicKey
        )

        status.value = 'ok'
        form.value = { name: '', email: '', mobile: '', message: '' } // reset
    } catch (err) {
        console.error(err)
        status.value = 'err'
    } finally {
        sending.value = false
    }
}
</script>

<style scoped>
/* ---------- layout ---------- */
.page {
    --accent: #e02537;
    --radius: 0.75rem;
    font-family: system-ui, sans-serif;
    width: 100%;

    /* centre everything */
    min-height: 100vh;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    gap: 5rem;
    text-align: center;
    padding-top:2rem ;
}

/* ---------- hero ---------- */
.title {
    font-size: clamp(2rem, 4vw + 1rem, 3.5rem);
    margin-bottom: 0.3em;
}

.subtitle {
    font-size: 1.1rem;
    opacity: 0.8;
    max-width: 32rem;
    margin: 0.4em auto 1.4em;
}

/* ---------- contact ---------- */
.section-title {
    font-size: 2rem;
    margin-bottom: 1.5rem;
}

.contact {
    width: 100%;
    max-width: 500px;
    /* keeps the form tidy */
    padding: 2rem;
    border-radius: 30px;
    border: 2px solid darkcyan;
}

/* ---------- form ---------- */
.form {
    display: grid;
    gap: 1.2rem;
    width: 100%;
}

.field {
    display: flex;
    flex-direction: column;
    gap: 0.3rem;
}

/* use full width for inputs */
input,
textarea {
    font: inherit;
    width: 100%;
    padding: 0.7em 0.9em;
    border: 1px solid #ccc;
    border-radius: var(--radius);
    resize: none;
    /* 🔒 stop textarea resizing */
    box-sizing: border-box;
}

/* ---------- button ---------- */
.btn {
    background: var(--accent);
    color: #fff;
    font-weight: 600;
    padding: 0.8em 2.4em;
    border: none;
    border-radius: var(--radius);
    cursor: pointer;
    transition: opacity 0.2s;
}

.btn[disabled] {
    opacity: 0.6;
    cursor: not-allowed;
}

/* ---------- status messages ---------- */
.success {
    color: green;
    margin-top: 1rem;
}

.error {
    color: crimson;
    margin-top: 1rem;
}

@media (max-width: 480px) {
    .contact {
        padding: 10px;
        border-radius: 0;
        /* optional: flatten border for full width look */
        border: none;
        /* optional: remove border on mobile */
    }
}
footer {
  display: flex;
  width: 100%;
  justify-content: center;
  padding: 5px;
  background-color: #434344;
  color: #fff;
}
</style>
