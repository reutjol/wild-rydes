<script>
import profileForm from '@/components/profileForm.vue'
import footers     from '@/components/footer.vue'
import menu        from '@/components/menu.vue'
import { Auth }    from 'aws-amplify'
import AmplifyStore from '../store/store'

export default {
  name: 'Auth',
  components: { profileForm, footers, menus: menu },

 data() {
  return {
    profilePic: false,
    imagePath: '',
    photoPickerConfig: {
      header: 'Upload Profile Pic',
      accept: 'image/*',
      path: '',
      defaultName: 'avatar'
    },
    mfa: false,
    fields: [
      { type: 'string', name: 'email', label: 'Email' },
      { type: 'string', name: 'phone_number', label: 'Phone Number' }
    ],
  };
},

mounted() {
  // טען את המשתמש ברגע שהוא מוכן
  if (AmplifyStore.state.user) {
    const username = AmplifyStore.state.user.username;
    this.imagePath = `${username}/avatar`;
    this.photoPickerConfig.path = `${username}/`;
  } else {
    console.warn("User not loaded yet");
  }
},

  methods: {
    toggleAccordion (el) {
      this[el] = !this[el]
    }
  },

  computed: {
    mfaConfig () {
      return {
        mfaDescription: "My app's mfa description!!",
        mfaTypes: ['TOTP', 'SMS', 'None'],
        cancelHandler: () => this.toggleAccordion('mfa')
      }
    },
    user () {
      return AmplifyStore.state.user
    },
    profilePicAccordion () {
      return {
        'is-closed': !this.profilePic,
        'is-primary': this.profilePic,
        'is-dark': !this.profilePic
      }
    },
    mfaAccordion () {
      return {
        'is-closed': !this.mfa,
        'is-primary': this.mfa,
        'is-dark': !this.mfa
      }
    }
  }
}
</script>
