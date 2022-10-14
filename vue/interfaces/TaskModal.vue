<template>
  <a-modal :visible="toggleModal" footer @cancel="closeModal">
    <vuci-form uci-config="task_one" :key="counter" @applied="closeModal">
      <vuci-named-section
        uci-config="task_one"
        :name="sectionId"
        type="test"
        :collapsible="false"
        v-slot="{ s }"
        @change-protocol="event"
      >
        <vuci-form-item-dummy
          :uci-section="s"
          label="Interface name:"
          name="name"
        />
        <vuci-form-item-select
          :uci-section="s"
          label="Protocol"
          name="protocol"
          :options="['static', 'dhcp']"
          initial="dhcp"
        >
        </vuci-form-item-select>
        <vuci-form-item-input
          :uci-section="s"
          label="Address"
          name="address"
          rules="ipaddr"
          depend="protocol == 'static'"
        />
        <vuci-form-item-input
          :uci-section="s"
          label="Netmask"
          name="netmask"
          rules="netmask4"
          depend="protocol == 'static'"
        />
        <vuci-form-item-input
          :uci-section="s"
          label="Gateway"
          name="gateway"
          rules="ipaddr"
          depend="protocol == 'static'"
        />
        <vuci-form-item-list
          :uci-section="s"
          label="DNS"
          name="dns"
          depend="protocol == 'static'"
        />
      </vuci-named-section>
    </vuci-form>
  </a-modal>
</template>

<script>
export default {
  props: {
    toggleModal: {
      type: Boolean,
      required: true
    },
    counter: {
      type: Number,
      required: true
    },
    sectionId: {
      type: String,
      required: true
    }
  },
  methods: {
    closeModal () {
      this.$emit('closeModal')
    },
    event (self) {
      if (self.model === 'dhcp') {
        this.$uci.set('task_one', this.sectionId, 'address', '')
        this.$uci.set('task_one', this.sectionId, 'gateway', '')
        this.$uci.set('task_one', this.sectionId, 'dns', '')
        this.$uci.set('task_one', this.sectionId, 'netmask', '')
        // don't use this at home, it's a workaround
        this.$uci.set('task_one', this.sectionId, 'workaround', '')
      } else {
        this.$uci.reset()
      }
    }
  }
}
</script>
