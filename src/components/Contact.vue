<script setup>
    import { ref, onMounted, onBeforeMount } from 'vue';

    import { Notyf } from 'notyf';
    import 'notyf/notyf.min.css';

    const notyf = new Notyf();

    const WEB3FORMS_ACCESS_KEY = "1c28a6b3-1a75-438b-9ad5-cfd291b53fa0";

    const emailSubject  = "New message from Portfolio Contact Form";

    const name = ref("");
    const email = ref("");
		const subject = ref("");
    const message = ref("");

    const isLoading = ref(false);


    const submitForm = async() => {

        if(!recaptchaToken.value) {
            notyf.error('Please verify that you are not a robot.');
            return;
        }

        isLoading.value = true;

        try {

            const response = await fetch("https://api.web3forms.com/submit", {
                method: "POST",
                headers: {
                    "Content-Type": "application/json",
                    Accept: "application/json"
                },
                body: JSON.stringify({
                    access_key: WEB3FORMS_ACCESS_KEY,
                    subject: subject.value 
											? `${emailSubject} - ${subject.value}` 
											: emailSubject,
                    name: name.value,
                    email: email.value,
                    message: message.value
                })
            });

            const result = await response.json();

            if(result.success) {
                console.log(result)

                isLoading.value = false;
                notyf.success("Message sent!");
            }

        } catch(error) {
            console.log(error);

            isLoading.value = false;
            notyf.error("Failed to send message");

        } finally {

            resetRecaptcha();
        }
    }

    /*recaptcha integration*/
    // 6Ldn5_YsAAAAAKMODxn2LbKCv9b2gF2zK3tU3Ihx
    const SITE_KEY = '6LdgrP0sAAAAALGoO-96sOmvUtdnCZmnaJEOaihh';

    const recaptchaContainer = ref(null);
    const recaptchaWidgetId = ref(null);
    const recaptchaToken = ref('');


    function onRecaptchaSuccess(token) {
        recaptchaToken.value = token;
    }

    function onRecaptchaExpired() {
        recaptchaToken.value = '';
    }

    function renderRecaptcha() {
        if(!window.grecaptcha) {
            console.error('reCAPTCHA not loaded');
            return;
        }

        recaptchaWidgetId.value = window.grecaptcha.render(recaptchaContainer.value, {
            sitekey: SITE_KEY,
            size: 'normal',
            callback: onRecaptchaSuccess,
            'expired-callback': onRecaptchaExpired
        });
    }

    function resetRecaptcha() {
        if(recaptchaWidgetId.value !== null) {
            window.grecaptcha.reset(recaptchaWidgetId.value);
            recaptchaToken.value = '';
        }
    }

    onMounted(() => {
        const interval = setInterval(() => {
            if(window.grecaptcha && window.grecaptcha.render) {
                renderRecaptcha();
                clearInterval(interval)
            }
        }, 100);

        onBeforeMount(() => {
            clearInterval(interval);
        });
    })

</script>


<template>
  <div class="row p-3 p-md-5" id="contact">

		<!-- Section Title -->
		<div class="col-12 mb-4">
			<p class="section-label">Get In Touch</p>
			<h1><span class="title-white">Contact</span> <span class="title-teal">Me</span></h1>
		</div>

		<!-- Google Map -->
		<div class="col-12 col-md-6 mb-4">
			<iframe
				src="https://www.google.com/maps/embed?pb=!1m18!1m12!1m3!1d3881.1440512419367!2d121.17919768885497!3d13.403405800000005!2m3!1f0!2f0!3f0!3m2!1i1024!2i768!4f13.1!3m3!1m2!1s0x33bceed68a46e60b%3A0xf4ff22ec3a61a9f1!2sRobinsons%20Department%20Store%20Calapan!5e0!3m2!1sen!2sph!4v1776092223766!5m2!1sen!2sph"
				class="contact-map"
				allowfullscreen
				loading="lazy"
				referrerpolicy="no-referrer-when-downgrade">
			</iframe>
		</div>

		<!-- Contact Form -->
		<div class="col-12 col-md-6 mb-4">
			<form @submit.prevent="submitForm" >
				<!-- Name -->
				<div class="mb-3">
					<!-- <label for="contactName" class="form-label">Name</label> -->
					<input v-model="name" type="text" class="form-control" id="contactName" placeholder="Your Name">
				</div>

				<!-- Email -->
				<div class="mb-3">
					<!-- <label for="contactEmail" class="form-label">Email</label> -->
					<input v-model="email" type="email" class="form-control" id="contactEmail" placeholder="Your Email">
				</div>

				<!-- Subject -->
				<div class="mb-3">
					<!-- <label for="contactSubject" class="form-label">Subject</label> -->
					<input v-model="subject" type="text" class="form-control" id="contactSubject" placeholder="Subject">
				</div>

				<!-- Message -->
				<div class="mb-3">
					<!-- <label for="contactMessage" class="form-label">Message</label> -->
					<textarea v-model="message" class="form-control" id="contactMessage" rows="4" placeholder="Your Message"></textarea>
				</div>

				<!-- Submit Button -->
				<button :disabled="isLoading" type="submit" class="btn-contact">{{ isLoading ? "Sending..." : "Submit" }}</button>

				<div class="d-flex justify-content-end mt-2">
					<div ref="recaptchaContainer"></div>
				</div>
				<!-- Socials -->
				<div class="d-flex gap-2 mt-4">
					<a href="https://github.com/sirjareth" class="social-btn" title="GitHub" target="_blank">
						
						<img src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/github/github-original.svg" />
						
					</a>
					<a href="https://www.linkedin.com/feed/" class="social-btn" title="LinkedIn" target="_blank">
						<img src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/linkedin/linkedin-plain.svg" />
					</a>
					
				</div>
			</form>
		</div>

	</div>
</template>