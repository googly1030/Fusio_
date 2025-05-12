<template>
    <div class="flex h-screen bg-gray-50">
      <!-- Sidebar -->
      <!-- <aside 
        class="bg-blue-900 text-white transition-all duration-300 ease-in-out fixed h-full z-10 md:relative"
        :class="[sidebarCollapsed ? 'w-20' : 'w-64']"
      >
        <div class="flex items-center justify-center h-16 border-b border-blue-800">
          <div class="flex items-center" :class="[sidebarCollapsed ? 'justify-center' : 'px-4']">
            <div class="flex items-center space-x-2">
              <Building2 :size="28" class="text-white" />
              <span v-if="!sidebarCollapsed" class="text-xl font-bold">NOTEL</span>
            </div>
          </div>
        </div>
  
        <nav class="mt-6">
          <ul>
            <li v-for="item in menuItems" :key="item.id">
              <button
                @click="activeItem = item.id"
                class="flex items-center w-full p-4 transition-colors duration-200"
                :class="[
                  activeItem === item.id 
                    ? 'bg-blue-800 border-l-4 border-white' 
                    : 'hover:bg-blue-800',
                  sidebarCollapsed ? 'justify-center' : 'justify-start'
                ]"
              >
                <span class="text-blue-100">
                  <component :is="item.icon" />
                </span>
                <span v-if="!sidebarCollapsed" class="ml-4 text-sm font-medium">{{ item.label }}</span>
              </button>
            </li>
          </ul>
        </nav>
      </aside> -->
  
      <div class="flex flex-col flex-1 overflow-hidden">
        <!-- Header -->
        <header class="bg-white border-b border-gray-200 h-16 flex items-center justify-between px-4 md:px-6">
          <div class="flex items-center">
            <button 
              @click="sidebarCollapsed = !sidebarCollapsed"
              class="p-2 rounded-md text-gray-500 hover:bg-gray-100 focus:outline-none"
            >
              <Menu v-if="sidebarCollapsed" :size="20" />
              <X v-else :size="20" />
            </button>
            <h1 class="ml-4 text-xl font-semibold text-gray-800">Dashboard</h1>
          </div>
  
          <div class="hidden md:flex items-center bg-gray-100 rounded-md px-3 py-2 flex-1 max-w-md mx-4">
            <Search :size="18" class="text-gray-400" />
            <input
              type="text"
              placeholder="Search..."
              class="bg-transparent border-none outline-none ml-2 w-full text-sm text-gray-700"
            />
          </div>
  
          <div class="flex items-center">
            <button class="relative p-2 rounded-full text-gray-500 hover:bg-gray-100 focus:outline-none">
              <Bell :size="20" />
              <span class="absolute top-1 right-1 w-3 h-3 bg-red-500 rounded-full border-2 border-white"></span>
            </button>
            
            <div class="ml-4 flex items-center">
              <div class="h-8 w-8 rounded-full bg-blue-500 flex items-center justify-center text-white">
                <span class="text-sm font-medium">JD</span>
              </div>
              <div class="ml-2 hidden md:block">
                <div class="flex items-center">
                  <span class="text-sm font-medium text-gray-700">John Doe</span>
                  <ChevronDown :size="16" class="ml-1 text-gray-500" />
                </div>
              </div>
            </div>
          </div>
        </header>
  
        <!-- Main Content -->
        <main class="flex-1 overflow-y-auto p-4 md:p-6">
          <div class="space-y-6">
            <!-- Page Header -->
            <div class="flex justify-between items-center">
              <h2 class="text-2xl font-bold text-gray-800">Overview</h2>
              <div class="flex space-x-2">
                <select class="px-3 py-2 bg-white border border-gray-300 rounded-md text-sm text-gray-700 focus:outline-none focus:ring-2 focus:ring-blue-500">
                  <option>This Month</option>
                  <option>Last Month</option>
                  <option>Last Quarter</option>
                  <option>This Year</option>
                </select>
                <button class="px-4 py-2 bg-blue-600 text-white rounded-md text-sm font-medium hover:bg-blue-700 transition-colors focus:outline-none focus:ring-2 focus:ring-blue-500 focus:ring-offset-2">
                  Export
                </button>
              </div>
            </div>
  
            <!-- Stats Overview -->
            <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-4 md:gap-6">
              <div 
                v-for="(stat, index) in stats" 
                :key="index" 
                class="bg-white rounded-lg shadow-sm p-6 transform transition-transform duration-300 hover:scale-[1.02]"
              >
                <div class="flex justify-between items-start">
                  <div>
                    <h3 class="text-gray-500 text-sm font-medium">{{ stat.title }}</h3>
                    <div class="mt-2 flex items-baseline">
                      <p class="text-2xl font-semibold text-gray-900">{{ stat.value }}</p>
                      <span 
                        v-if="stat.trend" 
                        class="ml-2 text-sm font-medium"
                        :class="[stat.trendUp ? 'text-green-600' : 'text-red-600']"
                      >
                        {{ stat.trend }}
                      </span>
                    </div>
                    <p class="mt-1 text-sm text-gray-500">{{ stat.subtitle }}</p>
                  </div>
                  <div :class="[stat.iconBg, 'p-3 rounded-full']">
                    <component :is="stat.icon" :size="20" class="text-gray-700" />
                  </div>
                </div>
              </div>
            </div>
  
            <!-- Main Content Grid -->
            <div class="grid grid-cols-1 xl:grid-cols-3 gap-6">
              <!-- Projects Table -->
              <div class="xl:col-span-2">
                <div class="bg-white rounded-lg shadow-sm p-6">
                  <div class="flex justify-between items-center mb-6">
                    <h3 class="text-lg font-semibold text-gray-800">Projects</h3>
                    <button class="text-sm text-blue-600 hover:text-blue-800 font-medium">
                      View All
                    </button>
                  </div>
                  
                  <div>
                    <div class="mb-4 flex">
                      <div class="relative flex-1 max-w-xs">
                        <div class="absolute inset-y-0 left-0 flex items-center pl-3 pointer-events-none">
                          <Search :size="16" class="text-gray-400" />
                        </div>
                        <input
                          type="text"
                          class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-md focus:ring-blue-500 focus:border-blue-500 block w-full pl-10 p-2.5"
                          placeholder="Search projects..."
                          v-model="searchTerm"
                        />
                      </div>
                    </div>
                    
                    <div class="overflow-x-auto">
                      <table class="min-w-full divide-y divide-gray-200">
                        <thead class="bg-gray-50">
                          <tr>
                            <th class="px-4 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">
                              Work
                            </th>
                            <th class="px-4 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">
                              Work type
                            </th>
                            <th class="px-4 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">
                              # proposals
                            </th>
                            <th class="px-4 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">
                              Preferred proposal
                            </th>
                            <th class="px-4 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">
                              Contractor
                            </th>
                            <th class="px-4 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">
                              Status
                            </th>
                            <th class="px-4 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">
                              Deadline
                            </th>
                          </tr>
                        </thead>
                        <tbody class="bg-white divide-y divide-gray-200">
                          <tr 
                            v-for="project in filteredProjects" 
                            :key="project.id" 
                            class="hover:bg-blue-50 transition-colors"
                          >
                            <td class="px-4 py-4 whitespace-nowrap">
                              <span class="text-sm font-medium text-blue-600">{{ project.name }}</span>
                            </td>
                            <td class="px-4 py-4 whitespace-nowrap text-sm text-gray-700">
                              {{ project.type }}
                            </td>
                            <td class="px-4 py-4 whitespace-nowrap text-center text-sm text-gray-700">
                              {{ project.proposals }}
                            </td>
                            <td class="px-4 py-4 whitespace-nowrap text-sm text-gray-700">
                              {{ project.preferredProposal ? `$${project.preferredProposal.toLocaleString()}` : '-' }}
                            </td>
                            <td class="px-4 py-4 whitespace-nowrap text-sm text-gray-700">
                              {{ project.contractor || '-' }}
                            </td>
                            <td class="px-4 py-4 whitespace-nowrap">
                              <span 
                                class="px-2 inline-flex text-xs leading-5 font-semibold rounded-full"
                                :class="getStatusClass(project.status)"
                              >
                                {{ project.status }}
                              </span>
                            </td>
                            <td class="px-4 py-4 whitespace-nowrap text-sm text-gray-700">
                              <span :class="{ 'text-red-600 font-medium': project.isLate }">
                                {{ project.deadline }}
                              </span>
                            </td>
                          </tr>
                        </tbody>
                      </table>
                    </div>
                  </div>
                </div>
              </div>
  
              <!-- Chart Section -->
              <div>
                <div class="bg-white rounded-lg shadow-sm p-6">
                  <div class="flex justify-between items-center mb-6">
                    <h3 class="text-lg font-semibold text-gray-800">Summary</h3>
                    <div class="flex space-x-2">
                      <button
                        class="px-3 py-1.5 text-sm font-medium rounded-md"
                        :class="[
                          activeTab === 'bids'
                            ? 'bg-blue-600 text-white'
                            : 'bg-gray-100 text-gray-600 hover:bg-gray-200'
                        ]"
                        @click="activeTab = 'bids'"
                      >
                        Bids
                      </button>
                      <button
                        class="px-3 py-1.5 text-sm font-medium rounded-md"
                        :class="[
                          activeTab === 'proposals'
                            ? 'bg-blue-600 text-white'
                            : 'bg-gray-100 text-gray-600 hover:bg-gray-200'
                        ]"
                        @click="activeTab = 'proposals'"
                      >
                        Proposals
                      </button>
                    </div>
                  </div>
  
                  <div v-if="activeTab === 'bids'" class="relative pt-4">
                    <div class="aspect-square relative">
                      <div class="absolute inset-0 flex items-center justify-center">
                        <svg width="100%" height="100%" viewBox="0 0 42 42" class="transform -rotate-90">
                          <circle cx="21" cy="21" r="15.91549430918954" fill="transparent" stroke="#f3f4f6" stroke-width="3"></circle>
                          <circle cx="21" cy="21" r="15.91549430918954" fill="transparent" stroke="#8b5cf6" stroke-width="3"
                            stroke-dasharray="33.4 100" stroke-dashoffset="25"></circle>
                          <circle cx="21" cy="21" r="15.91549430918954" fill="transparent" stroke="#facc15" stroke-width="3"
                            stroke-dasharray="62.6 100" stroke-dashoffset="58.4"></circle>
                        </svg>
                        <div class="absolute flex items-center justify-center flex-col">
                          <span class="text-xs text-gray-500">Total</span>
                          <span class="font-bold text-lg">8</span>
                        </div>
                      </div>
                    </div>
                    
                    <div class="mt-6 space-y-2">
                      <div class="flex items-center">
                        <span class="h-3 w-3 rounded-full bg-yellow-400 mr-2"></span>
                        <span class="text-sm text-gray-600">Awaiting proposals - 5 (63%)</span>
                      </div>
                      <div class="flex items-center">
                        <span class="h-3 w-3 rounded-full bg-purple-500 mr-2"></span>
                        <span class="text-sm text-gray-600">Decision - 3 (37%)</span>
                      </div>
                    </div>
                  </div>
  
                  <div v-if="activeTab === 'proposals'" class="pt-4">
                    <div class="space-y-4">
                      <div>
                        <div class="flex justify-between mb-1 items-center">
                          <span class="text-sm font-medium text-gray-700">Air conditioning facilities</span>
                          <span class="text-sm font-medium text-gray-700">$4,500,000</span>
                        </div>
                        <div class="w-full bg-blue-100 rounded-full h-2.5">
                          <div class="bg-blue-600 h-2.5 rounded-full" style="width: 70%"></div>
                        </div>
                      </div>
                      
                      <div>
                        <div class="flex justify-between mb-1 items-center">
                          <span class="text-sm font-medium text-gray-700">Sanitation facilities</span>
                          <span class="text-sm font-medium text-gray-700">$4,000,000</span>
                        </div>
                        <div class="w-full bg-blue-100 rounded-full h-2.5">
                          <div class="bg-blue-600 h-2.5 rounded-full" style="width: 65%"></div>
                        </div>
                      </div>
                      
                      <div>
                        <div class="flex justify-between mb-1 items-center">
                          <span class="text-sm font-medium text-gray-700">Flooring</span>
                          <span class="text-sm font-medium text-gray-700">$34,525</span>
                        </div>
                        <div class="w-full bg-blue-100 rounded-full h-2.5">
                          <div class="bg-blue-600 h-2.5 rounded-full" style="width: 10%"></div>
                        </div>
                      </div>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </main>
      </div>
    </div>
  </template>
  
  <script setup>
  import { ref, computed } from 'vue';
  import { 
    LayoutDashboard, 
    FileText, 
    BarChart3, 
    Settings, 
    Users, 
    Building2, 
    MessageSquare,
    Bell, 
    Search, 
    Menu, 
    X,
    ChevronDown,
    TrendingUp,
    DollarSign
  } from 'lucide-vue-next';
  
  const sidebarCollapsed = ref(false);
  const activeItem = ref('dashboard');
  const searchTerm = ref('');
  const activeTab = ref('bids');

  const projects = [
    {
      id: 1,
      name: "Earthwork",
      type: "Earthworks",
      proposals: "2/2",
      preferredProposal: null,
      contractor: null,
      status: "Awaiting proposals",
      deadline: "05.05.2022 10:00",
      isLate: true
    },
    {
      id: 2,
      name: "Sealing",
      type: "Sealing works",
      proposals: "1/1",
      preferredProposal: null,
      contractor: null,
      status: "Awaiting proposals",
      deadline: "09.04.2023 10:00",
      isLate: false
    },
    {
      id: 3,
      name: "Sanitation facilities",
      type: "Sanitation facilities",
      proposals: "4/4",
      preferredProposal: 4000000,
      contractor: "Sosa Projects",
      status: "Decision",
      deadline: "09.05.2023 10:00",
      isLate: false
    },
    {
      id: 4,
      name: "Electrical work",
      type: "Electrical works",
      proposals: "2/2",
      preferredProposal: null,
      contractor: null,
      status: "Awaiting proposals",
      deadline: "09.01.2024 10:00",
      isLate: false
    },
    {
      id: 5,
      name: "Flooring",
      type: "Flooring and cladding",
      proposals: "3/3",
      preferredProposal: 34525,
      contractor: "Adar Projects",
      status: "Decision",
      deadline: "12.01.2023 10:00",
      isLate: true
    },
    {
      id: 6,
      name: "Coloring works",
      type: "Paint works",
      proposals: "4/4",
      preferredProposal: null,
      contractor: null,
      status: "Awaiting proposals",
      deadline: "13.01.2023 10:00",
      isLate: true
    },
    {
      id: 7,
      name: "Aluminum",
      type: "Aluminum works",
      proposals: "2/2",
      preferredProposal: null,
      contractor: null,
      status: "Awaiting proposals",
      deadline: "21.02.2023 10:00",
      isLate: true
    },
    {
      id: 8,
      name: "Air conditioning facilities",
      type: "Air conditioning facilities",
      proposals: "1/1",
      preferredProposal: 4500000,
      contractor: "Uri Levy & Sons Ltd",
      status: "Decision",
      deadline: "15.01.2023 12:00",
      isLate: true
    }
  ];

  const stats = [
    {
      title: "Approved",
      value: "76%",
      subtitle: "19 / 25",
      icon: TrendingUp,
      iconBg: "bg-orange-100",
      trend: "+5.2%",
      trendUp: true
    },
    {
      title: "Proposals",
      value: "76%",
      subtitle: "19 / 25",
      icon: FileText,
      iconBg: "bg-orange-100",
      trend: "+2.3%",
      trendUp: true
    },
    {
      title: "Chosen Contractor",
      value: "3",
      subtitle: "3 / 8",
      icon: Users,
      iconBg: "bg-red-100",
      trend: "-1.5%",
      trendUp: false
    },
    {
      title: "Preferred Proposals",
      value: "$8,534,525",
      subtitle: "-9% from average",
      icon: DollarSign,
      iconBg: "bg-green-100",
      trend: "+12.1%",
      trendUp: true
    }
  ];

  const menuItems = [
    { id: 'dashboard', icon: LayoutDashboard, label: 'Dashboard' },
    { id: 'projects', icon: Building2, label: 'Projects' },
    { id: 'proposals', icon: FileText, label: 'Proposals' },
    { id: 'analytics', icon: BarChart3, label: 'Analytics' },
    { id: 'messages', icon: MessageSquare, label: 'Messages' },
    { id: 'team', icon: Users, label: 'Team' },
    { id: 'settings', icon: Settings, label: 'Settings' },
  ];

  const filteredProjects = computed(() => {
    return projects.filter(project => 
      project.name.toLowerCase().includes(searchTerm.value.toLowerCase())
    );
  });

  const getStatusClass = (status) => {
    switch (status) {
      case 'Awaiting proposals':
        return 'bg-amber-100 text-amber-800';
      case 'Decision':
        return 'bg-purple-100 text-purple-800';
      case 'In progress':
        return 'bg-blue-100 text-blue-800';
      case 'Completed':
        return 'bg-green-100 text-green-800';
      default:
        return 'bg-gray-100 text-gray-800';
    }
  };
  </script>