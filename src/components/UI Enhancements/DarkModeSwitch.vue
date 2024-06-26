<!-- Dark mode switch with local storage saving as well as system preference. -->

<template>
    <div class="flex-1 flex justify-end">
        <div class="switch mr-5">
            <input type="checkbox" class="switch__input" id="darkModeToggle" v-model="isDarkModeReversed">
            <label class="switch__label" for="darkModeToggle">
                <span class="switch__indicator"></span>
                <span class="switch__decoration"></span>
            </label>
        </div>
    </div>
</template>

<script>
import { computed, onMounted, ref, watch } from 'vue';
export default {
    setup() {
    const prefersDarkMode = window.matchMedia && window.matchMedia('(prefers-color-scheme: dark)').matches;
    const savedDarkMode = window.localStorage.getItem('isDarkMode');
    const initialDarkMode = savedDarkMode !== null ? savedDarkMode === 'true' : prefersDarkMode;

    const isDarkMode = ref(initialDarkMode);

    watch(isDarkMode, (newValue) => {
        console.log("isDarkMode changed:", newValue);
        if (newValue) {
        document.documentElement.classList.add('dark');
        } else {
        document.documentElement.classList.remove('dark');
        }
        window.localStorage.setItem('isDarkMode', newValue.toString());
    });

    onMounted(() => {
        const savedDarkMode = window.localStorage.getItem('isDarkMode');
        if (savedDarkMode !== null) {
            isDarkMode.value = savedDarkMode === 'true';
        } else {
            const prefersDarkMode = window.matchMedia && window.matchMedia('(prefers-color-scheme: dark)').matches;
            isDarkMode.value = prefersDarkMode;
        }

        if (isDarkMode.value) {
            document.documentElement.classList.add('dark');
        } else {
            document.documentElement.classList.remove('dark');
        }
    });

    const isDarkModeReversed = computed({
        get: () => !isDarkMode.value,
        set: (val) => { isDarkMode.value = !val; }
    });

    return {
        prefersDarkMode,
        isDarkModeReversed
    };
    }
}
</script>

<style>
body {
  padding: 50px;
}

.switch {
    display: inline-block;
    position: relative;
    transform: scale(0.5);
    transform-origin: top left;
}


.switch__input {
    clip: rect(1px, 1px, 1px, 1px);
    clip-path: inset(50%);
    height: 1px;
    width: 1px;
    margin: -1px;
    overflow: hidden;
    padding: 0;
    position: absolute;
}

.switch__label {
    position: relative;
    display: inline-block;
    width: 120px;
    height: 60px;
    background-color: #2B2B2B;
    border: 5px solid #5B5B5B;
    border-radius: 9999px;
    cursor: pointer;
    transition: all 0.4s cubic-bezier(.46,.03,.52,.96);
}

.switch__indicator {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%) translateX(-72%);
    display: block;
    width: 40px;
    height: 40px;
    background-color: #7B7B7B;
    border-radius: 9999px;
    box-shadow: 10px 0px 0 0 rgba(#000000, 0.2) inset;

    &::before,
    &::after {
        position: absolute;
        content: '';
        display: block;
        background-color: #FFFFFF;
        border-radius: 9999px;
    }

    &::before {
        top: 7px;
        left: 7px;
        width: 9px;
        height: 9px;
        background-color: #FFFFFF;
        opacity: 0.6;
    }

    &::after {
        bottom: 8px;
        right: 6px;
        width: 14px;
        height: 14px;
        background-color: #FFFFFF;
        opacity: 0.8;
    }
}

.switch__decoration {
    position: absolute;
    top: 65%;
    left: 50%;
    display: block;
    width: 5px;
    height: 5px;
    background-color: #FFFFFF;
    border-radius: 9999px;
    animation: twinkle 0.8s infinite -0.6s;

    &::before,
    &::after {
        position: absolute;
        display: block;
        content: '';
        width: 5px;
        height: 5px;
        background-color: #FFFFFF;
        border-radius: 9999px;
    }

    &::before {
        top: -20px;
        left: 10px;
        opacity: 1;
        animation: twinkle 0.6s infinite;
    }

    &::after {
        top: -7px;
        left: 30px;
        animation: twinkle 0.6s infinite -0.2s;
    }
}

@keyframes twinkle {
    50% { opacity: 0.2; }
}

.switch__indicator {
    &,
    &::before,
    &::after {
        transition: all 0.4s cubic-bezier(.46,.03,.52,.96);
    }
}

.switch__input:checked + .switch__label {
    background-color: #8FB5F5;
    border-color: #347CF8;

    .switch__indicator {
        background-color: #ECD21F;
        box-shadow: none;
        transform: translate(-50%, -50%) translateX(72%);

        &::before,
        &::after {
            display: none;
        }
    }

    .switch__decoration {
        top: 50%;
        transform: translate(0%, -50%);
        animation: cloud 8s linear infinite;

        width: 20px;
        height: 20px;

        &::before {
            width: 10px;
            height: 10px;
            top: auto;
            bottom: 0;
            left: -8px;
            animation: none;
        }

        &::after {
            width: 15px;
            height: 15px;
            top: auto;
            bottom: 0;
            left: 16px;
            animation: none;
        }

        &,
        &::before,
        &::after {
            border-radius: 9999px 9999px 0 0;
        }

        &::after {
            border-bottom-right-radius: 9999px;
        }
    }
}

@keyframes cloud {
    0% {
        transform: translate(0%, -50%);
    }
    50% {
        transform: translate(-50%, -50%);
    }
    100% {
        transform: translate(0%, -50%);
    }
}
</style>