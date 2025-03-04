<template>
    <div v-if='user' class="min-h-screen bg-center bg-cover bg-no-repeat bg-[url('/csirf-background.png')] bg-gray-500 bg-blend-multiply p-6">
        <h1 class="text-4xl font-bold text-center mb-4 text-white">Dashboard</h1>
        <div class="container mx-auto bg-white rounded-lg shadow">
            <!-- Tabs -->
            <div class="border-b">
                <nav class="-mb-px flex space-x-4 px-4">
                    <button
                        :class="tab === 'jobs' ? activeTabClass : inactiveTabClass"
                        @click.prevent="tab = 'jobs'"
                    >Jobs
                    </button>
                    <button
                        v-if="user.role == 'company' || user.role == 'admin'"
                        :class="tab === 'students' ? activeTabClass : inactiveTabClass"
                        @click.prevent="tab = 'students'"
                    >{{ user.role === 'company' ? 'Applicants' : 'Students' }}
                    </button>
                </nav>
            </div>

            <!-- Filters -->
            <div class="flex justify-between items-center p-4">
                <div>
                    <input v-if="tab === 'jobs'"
                        v-model="jobSearch"
                        type="text"
                        placeholder="Search Jobs..."
                        class="border rounded px-4 py-2 text-black"
                    />
                    <input v-else
                        v-model="studentSearch"
                        type="text"
                        :placeholder="user.role == 'company' ? 'Search Applicants...' : 'Search Students...'"
                        class="border rounded px-4 py-2 text-black"
                    />
                    <select v-model="jobTypeFilter" v-if="tab === 'jobs'" class="border rounded px-4 py-2 text-black ml-2">
                        <option value="" disabled>Type</option>
                        <option class="hover:bg-[#acacad] hover:text-white" value="Full-Time">Full-Time</option>
                        <option class="hover:bg-[#acacad] hover:text-white" value="Part-Time">Part-Time</option>
                        <option class="hover:bg-[#1E1B4B] hover:text-white" value="Internship">Internship</option>
                    </select>
                    <select v-model="jobModeFilter" v-if="tab === 'jobs'" class="border rounded px-4 py-2 text-black ml-2">
                        <option value="" disabled>Mode</option>
                        <option value="On-Site">On-Site</option>
                        <option value="Hybrid">Hybrid</option>
                        <option value="Remote">Remote</option>
                    </select>
                    <select v-model="studentYearFilter" v-if="tab === 'students'" class="border rounded px-4 py-2 text-black ml-2">
                        <option value="" disabled>Year</option>
                        <option v-for="year in [1, 2, 3, 4]" :key="year" :value="year">
                            Year {{ year }}
                        </option>
                    </select>
                    <select
                        v-if="tab === 'students' && user.role === 'admin'"
                        v-model="eventFilter"
                        class="border rounded px-4 py-2 text-black ml-2"
                    >
                        <option
                            v-for="event in events"
                            :key="event.id"
                            :value="event.id"
                        >
                            {{ event.title }}
                        </option>
                    </select>
                    <select
                        v-model="itemsPerPage"
                        class="border rounded px-4 py-2 text-black ml-2"
                    >
                        <option v-for="number in [10, 15, 20, 25]" :key="number" :value="number">
                            {{ number }} Entries
                        </option>
                    </select>
                    <button
                        @click.prevent="refresh"
                        class="ml-2 px-4 py-2 bg-[#1E1B4B] text-white hover:bg-[#4e4eaa] rounded"
                    >
                        Refresh
                    </button>
                </div>
                <div class="flex items-center">
                    <button
                        v-if="tab === 'jobs'"
                        @click.prevent="addJob"
                        class="mr-2 px-4 py-2 bg-[#1E1B4B] text-white hover:bg-[#4e4eaa] rounded"
                    >
                        Add Job
                    </button>
                    <button
                        @click.prevent="exportToCSV"
                        class="px-4 py-2 bg-[#1E1B4B] text-white hover:bg-[#4e4eaa] rounded"
                    >
                        Export CSV
                    </button>
                </div>
            </div>

            <!-- Jobs Table -->
            <div v-if="tab === 'jobs'">
                <table class="w-full table-auto border-collapse border">
                    <thead class="text-white">
                        <tr class="bg-[#1E1B4B]">
                            <th class="p-2 border border-white">No.</th>
                            <th v-if="user.role === 'admin'" class="p-2 border border-white">
                                Company
                                <button @click.prevent="sort('companyName')" class="ml-2">
                                    <span :class="getSortIcon('companyName')">▲</span>
                                </button>
                            </th>
                            <th class="p-2 border border-white">
                                Position
                                <button @click.prevent="sort('position')" class="ml-2">
                                    <span :class="getSortIcon('position')">▲</span>
                                </button>
                            </th>
                            <th class="p-2 border border-white">Type</th>
                            <th class="p-2 border border-white">Mode</th>
                        </tr>
                    </thead>
                    <tbody class="text-black">
                        <tr
                            v-for="(job, index) in paginatedJobs"
                            :key="job.id"
                            class="text-center hover:bg-[#4e4eaa] hover:text-white cursor-pointer"
                            @click.prevent="openEditModal(job)"
                        >
                            <td class="p-2 border border-[#1E1B4B]">{{ index + 1 }}</td>
                            <td v-if="user.role === 'admin'" class="p-2 border border-[#1E1B4B]">{{ job.companyName }}</td>
                            <td class="p-2 border border-[#1E1B4B]">{{ job.position }}</td>
                            <td class="p-2 border border-[#1E1B4B]">{{ job.type }}</td>
                            <td class="p-2 border border-[#1E1B4B]">{{ job.mode }}</td>
                        </tr>
                    </tbody>
                </table>
                <Pagination
                    :currentPage="jobPage"
                    :totalPages="jobTotalPages"
                    :totalEntries="filteredJobs.length"
                    :itemsPerPage="itemsPerPage"
                    @change="changeJobPage"
                />
            </div>

            <!-- Students Table -->
            <div v-if="tab === 'students'">
                <table class="w-full table-auto border-collapse border">
                    <thead class="text-white">
                        <tr class="bg-[#1E1B4B]">
                            <th class="p-2 border border-white">No.</th>
                            <th class="p-2 border border-white">
                                Name
                                <button @click.prevent="sort('name')" class="ml-2">
                                    <span :class="getSortIcon('name')">▲</span>
                                </button>
                            </th>
                            <th class="p-2 border border-white">
                                Year
                                <button @click.prevent="sort('year')" class="ml-2">
                                    <span :class="getSortIcon('year')">▲</span>
                                </button>
                            </th>
                            <th class="p-2 border border-white">Email</th>
                            <th class="p-2 border border-white">Phone</th>
                            <th v-if="user.role === 'company'" class="border border-white">Resume Link</th>
                            <th v-if="user.role === 'company'" class="border border-white">Job Applied</th>
                        </tr>
                    </thead>
                    <tbody class="text-black">
                        <tr
                            v-for="(student, index) in paginatedStudents"
                            :key="student.id"
                            class="text-center"
                        >
                            <td class="p-2 border border-[#1E1B4B]">{{ index + 1 }}</td>
                            <td class="p-2 border border-[#1E1B4B]">{{ student.name }}</td>
                            <td class="p-2 border border-[#1E1B4B]">{{ student.year }}</td>
                            <td class="p-2 border border-[#1E1B4B]">{{ student.email }}</td>
                            <td class="p-2 border border-[#1E1B4B]">{{ student.phone }}</td>
                            <td v-if="user.role === 'company'" class="p-2 border border-[#1E1B4B]">
                                <a
                                    v-if="student.resume"
                                    :href="student.resume"
                                    target="_blank"
                                    class="text-blue-500 hover:underline hover:text-blue-700 hover:cursor-pointer"
                                >
                                    View Resume
                                </a>
                                <span v-else>-</span>
                            </td>

                            <td v-if="user.role === 'company'" class="p-2 border border-[#1E1B4B]">
                                <span v-if="student.appliedJobNames && student.appliedJobNames.length > 0">
                                    {{ student.appliedJobNames.join('') }}
                                    <!-- {{ student.appliedJobNames.join(student.appliedJobNames.length > 1 ? ', ' : '') }} -->
                                </span>
                                <span v-else>-</span>
                            </td>
                        </tr>
                    </tbody>
                </table>
                <Pagination
                    :currentPage="studentPage"
                    :totalPages="studentTotalPages"
                    :totalEntries="filteredStudents.length"
                    :itemsPerPage="itemsPerPage"
                    @change="changeStudentPage"
                />
            </div>
        </div>

        <EditModal
            v-if="isModalOpen && user"
            :item="selectedItem"
            :companies="companies"
            @close="closeEditModal"
            @update="updateItem"
        />
    </div>
</template>

<script>
import EditModal from '@/components/EditModal.vue';
import Pagination from '@/components/Pagination.vue';
import { auth, db } from '@/firebase';
import { onAuthStateChanged } from 'firebase/auth';
import { collection, doc, getDoc, getDocs, addDoc, updateDoc, deleteDoc, query, where, orderBy } from 'firebase/firestore';
import { serverTimestamp } from "firebase/firestore";

export default {
    components:{
        EditModal, Pagination
    },
    data() {
        return {
            user: null,
            jobSearch: '',
            studentSearch: '',
            jobTypeFilter: '',
            jobModeFilter: '',
            studentYearFilter: '',
            eventFilter: '',
            itemsPerPage: 10,

            // Data
            tab: 'jobs',
            jobs: [],
            students: [],
            companies: [],
            events: [],

            // Pagination
            jobPage: 1,
            studentPage: 1,

            // Others
            sortKey: '',
            sortOrder: 1,
            isModalOpen: false,
            selectedItem: null,
            loading: true,
        };
    },
    computed: {
        filteredJobs() {
            return this.jobs.filter((job) => {
                return (
                    (job.companyName.toLowerCase().includes(this.jobSearch.toLowerCase().trim()) ||
                    job.position.toLowerCase().includes(this.jobSearch.toLowerCase().trim())) &&
                    (this.jobTypeFilter ? job.type === this.jobTypeFilter : true) &&
                    (this.jobModeFilter ? job.mode === this.jobModeFilter : true)
                );
            });
        },
        paginatedJobs() {
            const start = (this.jobPage - 1) * this.itemsPerPage;
            return this.filteredJobs.slice(start, start + this.itemsPerPage);
        },
        jobTotalPages() {
            return Math.ceil(this.filteredJobs.length / this.itemsPerPage);
        },
        filteredStudents() {
            return this.students
                .filter((student) => {
                    if (this.user.role == "company") {
                        return  student.appliedJobs !== undefined && student.appliedJobs !== null;
                    }
                    else if (this.user.role === 'admin') {
                        return this.eventFilter
                            ? student.eventID === this.eventFilter
                            : true; // Filter by selected event
                    }
                    return true;    // Skip the filter for other roles
                })
                .filter((student) => {
                    return (
                        (
                            (this.user.role === 'company' && student.appliedJobNames &&
                                student.appliedJobNames.some((jobName) =>
                                    jobName.toLowerCase().includes(this.studentSearch.toLowerCase().trim())
                                )) ||
                            student.name.toLowerCase().includes(this.studentSearch.toLowerCase().trim()) ||
                            student.email.toLowerCase().includes(this.studentSearch.toLowerCase().trim()) ||
                            student.phone.toLowerCase().includes(this.studentSearch.toLowerCase().trim())
                        ) &&
                        (this.studentYearFilter ? student.year === this.studentYearFilter : true)
                    );
                });
        },
        paginatedStudents() {
            const start = (this.studentPage - 1) * this.itemsPerPage;
            return this.filteredStudents.slice(start, start + this.itemsPerPage);
        },
        studentTotalPages() {
            return Math.ceil(this.filteredStudents.length / this.itemsPerPage);
        },
        activeTabClass() {
            return 'text-[#1E1B4B] border-b-2 border-[#1E1B4B] py-2';
        },
        inactiveTabClass() {
            return 'text-gray-500 hover:text-[#1E1B4B] py-2';
        },
    },
    methods: {
        async fetchJobs() {
            const companySnapshot = await getDocs(collection(db, 'companies'));
            const companyMap = companySnapshot.docs.reduce((map, doc) => {
                map[doc.id] = doc.data().name; // Map companyID to companyName
                return map;
            }, {});

            let jobQuery = '';
            if (this.user.role == 'company') {
                jobQuery = query(collection(db, 'jobs'), where('companyID', '==', this.user.companyID), orderBy('createdAt', 'desc'));
            }
            else {
                jobQuery = query(collection(db, 'jobs'), orderBy('createdAt', 'desc'));
            }

            const jobSnapshot = await getDocs(jobQuery);
            this.jobs = jobSnapshot.docs
                .map((doc) => {
                    const jobData = doc.data();
                    return {
                        id: doc.id,
                        ...jobData,
                        companyName: companyMap[jobData.companyID] || '', // Map companyID to name
                    };
                })
        },
        async fetchStudents() {
            if (this.user.role == 'company') {
                // Fetch all jobs and create a map of job IDs to job names
                const jobQuery = query(
                    collection(db, 'jobs'),
                    where('companyID', '==', this.user.companyID)
                );
                const jobSnapshot = await getDocs(jobQuery);
                const jobMap = jobSnapshot.docs.reduce((map, doc) => {
                    map[doc.id] = doc.data().position; // Map job ID to job position name
                    return map;
                }, {});

                // Fetch all students
                const querySnapshot = await getDocs(collection(db, 'users'));
                this.students = querySnapshot.docs
                    .map((doc) => {
                        const studentData = doc.data();

                        // Map appliedJobs field to job names
                        const appliedJobNames = studentData.appliedJobs
                            ? studentData.appliedJobs.map((jobId) => jobMap[jobId] || '')
                            : [];

                        return {
                            id: doc.id,
                            ...studentData,
                            appliedJobNames, // Add the resolved job names to the student object
                            resume: studentData.resume ? studentData.resume.fileURL : null, // Add resume file URL
                        };
                    })
                    .filter((student) => student.appliedJobs && student.appliedJobs.length > 0); // Filter students who have appliedJobs
            } 
            else if (this.user.role == 'admin') {
                // Fetch all events
                const eventSnapshot = await getDocs(collection(db, 'events'));
                this.events = eventSnapshot.docs.map((doc) => ({                
                    id: doc.id,
                    title: doc.data().title,
                }));
                
                // Automatically set the first event as the default filter if available
                if (this.events.length > 0) {
                    this.eventFilter = this.events[0].id;
                }

                // Fetch all users who registered for events
                const querySnapshot = await getDocs(collection(db, 'users'));
                this.students = querySnapshot.docs.flatMap((doc) => {
                    const studentData = doc.data();

                    if (studentData.role != 'student' || !studentData.registeredEvents || !Array.isArray(studentData.registeredEvents)) {
                        return []; // Skip users without registeredEvents
                    }
                    
                    // Create a row for each event the student is registered for
                    return studentData.registeredEvents.map((eventID) => ({
                        id: doc.id,
                        ...studentData,
                        eventID,
                    }));
                });
            }
        },
        async fetchCompanies() {
            const querySnapshot = await getDocs(collection(db, 'companies'));
            this.companies = querySnapshot.docs.map(doc => ({
                id: doc.id,
                name: doc.data().name,
            }));
        },
        openEditModal(item) {
            if (!item) {
                console.error('Error Occurred');
                return;
            }
            this.selectedItem = { ...item };
            this.isModalOpen = true;
        },
        closeEditModal() {
            this.isModalOpen = false;
        },
        sort(key) {
            if (this.sortKey === key) {
                this.sortOrder *= -1;
            } else {
                this.sortKey = key;
                this.sortOrder = 1;
            }

            (this.tab == 'jobs' ? this.jobs : this.students).sort((a, b) => {
                if (a[key] > b[key]) return this.sortOrder;
                if (a[key] < b[key]) return -this.sortOrder;
                return 0;
            });
        },
        getSortIcon(key) {
            if (this.sortKey !== key) return '';
            return this.sortOrder === 1 ? '▲' : '▼';
        },
        changeJobPage(page) {
            this.jobPage = page;
        },
        changeStudentPage(page) {
            this.studentPage = page;
        },
        addJob() {
            this.selectedItem = this.user.role == 'admin' ? {companyID:'', position: '', description: '', type: '', mode: '', createdAt: serverTimestamp()} : {companyID: this.user.companyID, position: '', description: '', type: '', mode: '', createdAt: serverTimestamp()};
            this.isModalOpen = true;
        },
        async updateItem(updatedItem) {
            if (updatedItem.delete) {
                await deleteDoc(doc(db, 'jobs', updatedItem.id));
                this.jobs = this.jobs.filter(job => job.id !== updatedItem.id);
                this.isModalOpen = false;
                return;
            }

            if (this.tab === 'jobs') {
                if (!updatedItem.position || !updatedItem.type || !updatedItem.mode) {
                    alert('Please fill in all required fields');
                    return;
                }
                else if (updatedItem.id) {
                    // Exclude 'companyName' and 'id' before saving to Firestore
                    const { companyName, id, ...companyData } = updatedItem;

                    await updateDoc(doc(db, 'jobs', updatedItem.id), companyData);
                    this.isModalOpen = false;
                }
                else {
                    // Exclude 'companyName' and 'id' before saving to Firestore
                    const { companyName, id, ...companyData } = updatedItem;

                    const docRef = await addDoc(collection(db, 'jobs'), companyData);
                    this.jobs.push({ id: docRef.id, ...companyData });
                    this.isModalOpen = false;
                }
            }

            this.fetchJobs();
        },
        refresh() {
            this.jobSearch = ''
            this.studentSearch = ''
            this.jobTypeFilter = ''
            this.jobModeFilter = ''
            this.studentYearFilter = ''
            this.itemsPerPage = 10
            this.jobPage =  1
            this.studentPage =  1
            this.sortKey = ''
            this.sortOrder = ''
        },
        exportToCSV() {
            const rows = this.tab === 'jobs' ? this.filteredJobs : this.filteredStudents;

            if (!rows.length) {
                alert('No data to export');
                return;
            }

            const headers = this.tab === 'jobs'
                ? (this.user.role == 'company' ? ['Position', 'Type', 'Mode'] : ['Company', 'Position', 'Type', 'Mode'])
                : (this.user.role == 'company' ? ['Name', 'Year', 'Email', 'Phone', 'Resume'] : ['Name', 'Year', 'Email', 'Phone']);

            const data = rows.map((row) => {
                if (this.tab === 'jobs') {
                    return this.user.role == 'company' ? [row.position, row.type, row.mode] : [row.companyName, row.position, row.type, row.mode];
                } else {
                    if (this.user.role == 'admin') {
                        return [row.name, row.year, row.email, row.phone || '-'];
                    }
                    else {
                        return [row.name, row.year, row.email, row.phone, row.resume || '-'];
                    }
                }
            });

            const csvContent = [
                headers.join(','), // Add headers as the first row
                ...data.map((row) => row.map((value) => `"${value}"`).join(',')) // Add data rows
            ].join('\n');

            const blob = new Blob([csvContent], { type: 'text/csv;charset=utf-8;' });
            const url = URL.createObjectURL(blob);

            const link = document.createElement('a');
            link.href = url;
            link.setAttribute('download', `${this.tab}_data.csv`);
            document.body.appendChild(link);
            link.click();
            document.body.removeChild(link);
        },
    },
    async created() {
        try {
            onAuthStateChanged(auth, async (currentUser) => {
                if (currentUser) {
                    const userDoc = await getDoc(doc(db, 'users', currentUser.uid));
                    if (userDoc.exists()) {
                        this.user = userDoc.data();

                        if (this.user.role == 'student') {
                            this.$router.push('/profile');
                        }
                        
                        await this.fetchJobs();
                        await this.fetchStudents();
                        await this.fetchCompanies();
                    }
                } else {
                    this.$router.push('/login');
                }
            });

        }
        catch (error){
            console.error("Failed to fetch data:", error);
        }
    },
};
</script>
