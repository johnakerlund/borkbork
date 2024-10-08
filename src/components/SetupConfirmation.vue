<!--
  - @copyright 2019 Christoph Wurst <christoph@winzerhof-wurst.at>
  -
  - @author 2019 Christoph Wurst <christoph@winzerhof-wurst.at>
  -
  - @license GNU AGPL version 3 or any later version
  -
  - This program is free software: you can redistribute it and/or modify
  - it under the terms of the GNU Affero General Public License as
  - published by the Free Software Foundation, either version 3 of the
  - License, or (at your option) any later version.
  -
  - This program is distributed in the hope that it will be useful,
  - but WITHOUT ANY WARRANTY; without even the implied warranty of
  - MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
  - GNU Affero General Public License for more details.
  -
  - You should have received a copy of the GNU Affero General Public License
  - along with this program.  If not, see <http://www.gnu.org/licenses/>.
  -->

  <template>
    <div class="setup-confirmation">
        <p class="setup-confirmation__secret">
            {{ t('twofactor_totp', 'Your new TOTP secret is:') }} {{ secret }}
        </p>
        <p>
            Download a 2FA Authenticator App on your phone: <br>
            <a href="https://apps.apple.com/us/app/microsoft-authenticator/id983156458">Apple App Store (iPhone) </a><br>
            <a href="https://play.google.com/store/apps/details?id=com.azure.authenticator">Google Play Store (Android) </a>
        </p>
            <p> Want to learn more? Information about <a href="https://liu.se/en/organisation/liu/uf/digit/two-step-verification">Two-Factor Authentication at Linköping University.</a></p>
        <p>
			{{
				t(
					'twofactor_totp',
					'For quick setup, scan this QR code with your TOTP app:'
				)
			}}
		</p>
		<QR :value="qrUrl" :options="{ width: 150 }" />
		<p>
			{{
				t(
					'twofactor_totp',
					'After you configured your app, enter a test code below to ensure everything works correctly:'
				)
			}}
		</p>
		<input id="totp-confirmation"
			v-model="confirmationCode"
			type="tel"
			minlength="6"
			maxlength="10"
			autocomplete="off"
			autocapitalize="off"
			:disabled="loading"
			:placeholder="t('twofactor_totp', 'Authentication code')"
			@keydown="onConfirmKeyDown">
		<input id="totp-confirmation-submit"
			type="button"
			:disabled="loading"
			:value="t('twofactor_totp', 'Verify')"
			@click="confirm">
	</div>
</template>

<script>
import QR from '@chenfengyuan/vue-qrcode'

export default {
	name: 'SetupConfirmation',
	components: {
		QR,
	},
	props: {
		loading: {
			type: Boolean,
			default: false,
		},
		secret: {
			type: String,
			required: true,
		},
		qrUrl: {
			type: String,
			required: true,
		},
		confirmation: {
			type: String,
			default: '',
		},
	},
	data() {
		return {
			confirmationCode: this.confirmation,
		}
	},
	watch: {
		confirmation(newVal) {
			this.confirmationCode = newVal
		},
	},
	methods: {
		confirm() {
			this.$emit('update:confirmation', this.confirmationCode)
			this.$emit('confirm')
		},
		onConfirmKeyDown(e) {
			if (e.which === 13) {
				this.confirm()
			}
		},
	},
}
</script>

<style lang="scss" scoped>
.setup-confirmation {
	&__secret {
		word-break: break-all;
	}
}
</style>
