<template>
    <div>
        <button ref="button" class="filter-button" @click="togglePopover">
            {{ selectedOption || defaultLabel }}
            <i class="fa-regular fa-chevron-down"></i>
        </button>
        <div v-show="showDropdown" class="popover" ref="popoverRef">
            <button
                v-for="option in options"
                :key="option"
                @click="selectOption(option)"
                class="option_button"
            >
                {{ option }}
            </button>
        </div>
    </div>
</template>

<script>
export default {
    props: {
        options: {
            type: Array,
            required: true,
        },
        defaultLabel: {
            type: String,
            default: "Select",
        },
    },
    data() {
        return {
            showDropdown: false,
            selectedOption: null,
        };
    },
    methods: {
        toggleDropdown() {
            this.showDropdown = !this.showDropdown;
        },
        selectOption(option) {
            this.selectedOption = option;
            this.showDropdown = false;
            this.$emit("update:selected", option, this.defaultLabel); // Emitting the custom event
        },
        togglePopover() {
            const [element] = document.getElementsByClassName(
                "main-content-wrapper"
            );
            if (this.showDropdown) {
                element.style.overflowY = "scroll";
                this.showDropdown = false;
                return;
            }

            //
            this.showDropdown = true;
            element.style.overflowY = "hidden";

            this.$nextTick(() => {
                // make sure the DOM is updated
                const buttonRect = this.$refs.button.getBoundingClientRect(); // assuming the ref on your button is "button"
                const popover = this.$refs.popoverRef;

                popover.style.left = `${buttonRect.left}px`;
                popover.style.top = `${buttonRect.bottom}px`;
            });
        },
    },
};
</script>

<style>
.filter-button {
    position: relative;
    border-radius: 3rem;
    border: 1px solid #2b5db6;
    color: #2b5db6;
    font-size: 1.2rem;
    padding: 0.5rem 3rem;
    margin: 0 1.2rem;
    /* Estilos adicionales si es necesario */
    height: 100%;
}

.fa-regular.fa-chevron-down {
    font-size: 0.9rem;
    position: absolute;
    top: 15px;
    right: 20px;
}

.popover {
    position: absolute;
    box-shadow: 0px 0px 8px 0px rgba(0, 0, 0, 0.2);
    -webkit-box-shadow: 0px 0px 8px 0px rgba(0, 0, 0, 0.2);
    -moz-box-shadow: 0px 0px 8px 0px rgba(0, 0, 0, 0.2);
    gap: 1rem;
    border-radius: 15px;
    padding: 0.5rem 1rem;
    margin-top: 0.5rem;
    display: flex;
    flex-wrap: wrap;
    max-width: 50%;
    justify-content: safe;
}

.option_button {
    padding: 5px 10px;
    border: 1px solid #2b5db6;
    border-radius: 15px;
}
</style>
