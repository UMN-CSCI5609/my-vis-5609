<script lang="ts">
	import * as d3 from 'd3';
	import { onMount } from 'svelte';

	const menuItems = [
		{
			title: 'canvas',
			href: 'https://canvas.umn.edu/courses/541245',
			external: true
		},
		{
			title: 'staff', 
			href: '#staff',
			external: false
		},
		{
			title: 'assignments',
			href: '#assignments',
			external: false
		},
		{
			title: 'Final Project Gallery',
			href: '#Gallery',
			external: false
		},
		{
			title: 'resources',
			href: '#resources',
			external: false
		}
	];

	type FinalProject = {
		id: string;
		title: string;
		members: string[];
		link: string;
		semester: string;
		teaser?: string;
	};

	let finalProjects: FinalProject[] = [];
	let finalProjectsLoading = true;
	let finalProjectsError = '';
	let selectedSemester = 'All';

	$: semesters = [
		'All',
		...Array.from(new Set(finalProjects.map((project) => project.semester).filter(Boolean)))
	];
	$: filteredProjects =
		selectedSemester === 'All'
			? finalProjects
			: finalProjects.filter((project) => project.semester === selectedSemester);

	async function loadFinalProjects() {
		finalProjectsLoading = true;
		finalProjectsError = '';
		try {
			const data = await d3.csv('./final-projects.csv', (row) => {
				return {
					id: row.id?.trim() ?? '',
					title: row.title?.trim() ?? '',
					members: row.members?.split(',').map((member) => member.trim()).filter(Boolean) ?? [],
					link: row.link?.trim() ?? '',
					semester: row.semester?.trim() ?? '',
					teaser: "./teasers/" + row.id?.trim().split('-')[0] + "/" + row.id?.trim() + ".png"
				} as FinalProject;
			});
			finalProjects = data.filter((project) => project.title);
		} catch (error) {
			finalProjectsError = 'Failed to load final projects.';
			console.error(error);
		} finally {
			finalProjectsLoading = false;
		}
	}

	onMount(loadFinalProjects);

	const assignments = [
		{
			title: 'A0 Setup',
			instruction: 'https://github.com/UMN-CSCI5609/Assignments-Instructions/tree/main/A0-Setup',
			demo: 'https://qianwen.info/my-vis-5609/A0',
			due: 'Feb 2, 2026',
			external: true
		},
		{
			title: 'A1 Visual Encoding',
			instruction: 'https://github.com/UMN-CSCI5609/Assignments-Instructions/tree/main/A1-Visual-Encoding-1',
			demo: 'https://qianwen.info/my-vis-5609/A1',
			due: 'Feb 16, 2026',
			external: true
		},
		{
			title: 'A2 Interactive Analysis',
			instruction: 'https://github.com/UMN-CSCI5609/Assignments-Instructions/tree/main/A2-Analysis',
			demo: 'https://qianwen.info/my-vis-5609/A2',
			due: 'Mar 2, 2026',
			external: true
		},
		{
			title: 'A3 3D Visualization',
			instruction: 'https://github.com/UMN-CSCI5609/Assignments-Instructions/tree/a15e26beb7b417f3838887819c6bd61f94ecfe36/A3-3D',
			demo: 'https://qianwen.info/my-vis-5609/A3',
			due: 'Mar 16, 2026',
			external: true

		},
		{
			title: 'A4 Scrollytelling',
			instruction: 'https://github.com/UMN-CSCI5609/Assignments-Instructions/tree/a15e26beb7b417f3838887819c6bd61f94ecfe36/A4-Scrolly',
			demo: 'https://qianwen.info/my-vis-5609/A4',
			due: 'Mar 30, 2026',
			external: true
		}
	];

	const datasets = [
		{
			name: 'Data Is Plural',
			link: 'https://www.data-is-plural.com/',
			des: 'A spreadsheet archive of public interest datasets curated by Jeremy Singer-Vine (with an accompanying newsletter).'
		},
		{
			name: 'dataCommons.org',
			link: 'https://datacommons.org/',
			des: 'Aggregates and harmonizes global open data so you can ask questions with a knowledge graph.'
		},
		{
			name: 'Civic Data Sets for the Pacific Northwest',
			link: 'https://www.civicdata.io/',
			des: 'A free open data platform built by Accela for government agencies and civic datasets.'
		},
		{
			name: 'R Datasets Archive',
			link: 'https://vincentarelbundock.github.io/Rdatasets/',
			des: 'An archive of datasets distributed with the R statistical language.'
		},
		{
			name: '30 Places to Find Open Data on the Web (Visual.ly)',
			link: 'https://visual.ly/community/Infographics/education/30-places-find-open-data-web',
			des: 'An infographic article listing 30 places to find open data on the web.'
		},
		{
			name: 'Office for National Statistics (UK)',
			link: 'https://www.ons.gov.uk/',
			des: 'A repository of detailed statistics about Great Britain and Northern Ireland.'
		},
		{
			name: 'World Bank Data Catalog',
			link: 'https://datacatalog.worldbank.org/',
			des: 'Find, download, and share World Bank development data.'
		},
		{
			name: 'Machine Learning Repository',
			link: 'https://archive.ics.uci.edu/',
			des: 'Large variety of maintained datasets.'
		},
		{
			name: 'TidyTuesday',
			link: 'https://github.com/rfordatascience/tidytuesday',
			des: 'A new dataset is shared every Tuesday since 2018.'
		},
		{
			name: 'Data.gov Catalog',
			link: 'https://catalog.data.gov/dataset/',
			des: 'A comprehensive source for U.S. government open data.'
		},
		{
			name: 'Kaggle',
			link: 'https://www.kaggle.com/datasets',
			des: 'Explore open datasets and machine learning projects.'
		},
		{
			name: 'Human Mortality Database',
			link: 'https://www.mortality.org/',
			des: 'A rich resource for demographic and mortality data.'
		},
		{
			name: 'BuzzFeedNews on GitHub',
			link: 'https://github.com/BuzzFeedNews',
			des: 'Open-source data, analysis, libraries, tools, and guides from BuzzFeed News.'
		}
	];


	const toolkits = [
    {
      name: 'D3',
      link: 'https://d3js.org/',
	  examples: 'https://observablehq.com/@d3/gallery',
	  des: 'JavaScript library for data-driven visualization and interaction.'
    },
	{
		name: 'Vega', 
		link: 'https://vega.github.io/vega/',
		examples: 'https://vega.github.io/vega/examples/',
		des: 'Declarative JSON grammar for building interactive visualizations.'
	}, 
	{
		name: 'Vega-Lite', 
		link: 'https://vega.github.io/vega-lite/',
		examples: 'https://vega.github.io/editor/#/examples/vega-lite/circle_natural_disasters',
		des: 'High-level declarative grammar for expressive charts.'
	}
  ]

</script>


<header class="site-header">
	<a class="brand" href="#home">
		CSCI 5609: Visualization 2026
	</a>
	<nav class="menu" aria-label="Primary">
		{#each menuItems as item}
			<div class="menu-block">
			<a
				class="menu-item"
				href={item.href}
				target={item.external ? '_blank' : undefined}
				rel={item.external ? 'noreferrer' : undefined}
			>
				{item.title}
			</a>
			</div>
		{/each}
	</nav>
</header>

<div class="page">
<main class="content">
	<section class="hero" id="home">
		<!-- <h1>CSCI 5609: Visualization</h1> -->
		<img src="./cat.png" alt="Visualization" width="400px" style="margin-bottom: 16px; margin-top: 16px; border-radius: 10px;">
		<!-- <p>
			A hands-on course on visual data analysis, storytelling, and interactive
			graphics.
		</p> -->
	</section>

	<section id="staff" class="staff">
		<h2 class="staff-title">Staff</h2>
		<div class="staff-grid">
			<div class="staff-card">
				<h3>Instructor</h3>
				<p><b>Name</b>: <a class="staff-link" href="https://qianwen.info/" target="_blank" rel="noreferrer">Qianwen Wang</a></p>
				<p><b>Email</b>: qianwen@umn.edu</p>
				<p><b>Office Hour</b>: Tuesday 12:00-1:00 pm, Shepherd Labs 319</p>
			</div>
			<div class="staff-card">
				<h3>TA</h3>
				<p><b>Name</b>: <a class="staff-link" href="https://ppphhhleo.github.io/" target="_blank" rel="noreferrer">Pan Hao</a></p>
				<p><b>Email</b>: pan00342@umn.edu</p>
				<p><b>Office Hour</b>: Wednesday 2:30-3:30 pm, Shepherd Labs 341</p>
			</div>
		</div>
	</section>

	<section class="assignments" id="assignments">
		<h2>Assignments</h2>
		Please refer to the following instructions and demos. All due dates are at midnight (CT).
		{#each assignments as assignment}
			<p><b>{assignment.title}</b>: 
				<a href={assignment.instruction} target="_blank" rel="noreferrer" class="link-item">Instruction</a>, <a href={assignment.demo} target="_blank" rel="noreferrer" class="demo-item">Demo</a>, due: {assignment.due}</p>
		{/each}
	</section>
</main>
</div>

	<section class="final-project" id="Gallery">
		<h2 style="max-width: 90%; margin: 0 auto; padding-bottom: 16px;"
		>Final Project Gallery</h2>
		<div class="semester-filter">
			<!-- <span>Filter by semester</span> -->
			<div class="semester-buttons" role="group" aria-label="Filter final projects by semester">
				{#each semesters as semester}
					<button
						type="button"
						class:active={selectedSemester === semester}
						on:click={() => (selectedSemester = semester)}
					>
						{semester}
					</button>
				{/each}
			</div>
		</div>
		{#if finalProjectsLoading}
			<p>Loading final projects...</p>
		{:else if finalProjectsError}
			<p class="error">{finalProjectsError}</p>
		{:else if filteredProjects.length === 0}
			<p>No final projects for this semester.</p>
		{:else}
			<div class="project-grid">
				{#each filteredProjects as project}
					<article class="project-card">
						{#if project.teaser}
							<img
								class="project-teaser"
								src={project.teaser}
								alt={`Teaser for ${project.title}`}
								loading="lazy"
							/>
						{/if}
						<h2 class="project-title" 
							on:click={() => window.open(project.link, '_blank')}
						>
						{project.title}</h2>
						
						{#if project.members.length}
							<p class="project-members">{project.members.join(', ')}</p>
						{/if}
					</article>
				{/each}
			</div>
		{/if}
	</section>


	<section class="resources" id="resources">
		<h2>Resources</h2>
		<h3>Toolkits</h3>
		<ul class="resources-list">
			{#each toolkits as toolkit}
				<li>
					<a href={toolkit.link} target="_blank" rel="noreferrer" class="demo-item">
						{toolkit.name}
					</a>
					{#if toolkit.examples}
						<a href={toolkit.examples} target="_blank" rel="noreferrer" class="link-item">
							[Examples]
						</a>
					{/if}
					{#if toolkit.des}
						- {toolkit.des}
					{/if}
				</li>
			{/each}
		</ul>
		<h3>Datasets</h3>
		<ul class="resources-list">
			{#each datasets as dataset}
				<li>
					<a href={dataset.link} target="_blank" rel="noreferrer" class="demo-item">
						{dataset.name}
					</a>
					{#if dataset.des}
						- {dataset.des}
					{/if}
				</li>
			{/each}
		</ul>
		<h3>Color</h3>
		<ul class="resources-list">
		<li>
			<a href='https://colorbrewer2.org/#type=sequential&scheme=BuGn&n=3' target="_blank" rel="noreferrer" class="link-item">
			Color Brewer
			</a>
			- Color advice for cartograph
		</li>
	</ul>
		
	</section>



<style>
	.page {
		min-height: 95vh;
		max-width: 70%;
		margin: 0 auto;
		padding: 0 32px;
	}

	.site-header {
		position: sticky;
		top: 0;
		z-index: 10;
		display: flex;
		align-items: center;
		justify-content: space-between;
		padding: 18px 0;
		gap: 16px;
		border-bottom: 1px solid #e5e5e5;
		background: #ffffff;
	}

	:global(section[id]) {
		scroll-margin-top: 60px;
	}

	.brand {
		display: inline-block;
		padding-left: 180px;
		font-size: 1.2rem;
		font-weight: 700;
		color: inherit;
		text-decoration: none;
	}

	.menu {
		display: flex;
		gap: 20px;
		flex-wrap: wrap;
		max-width: 90%;
		padding-right: 180px;
	}

	.menu-block {
		display: inline-block;
	}

	.menu-item {
		text-decoration: none;
		color: inherit;
		font-weight: 600;
		font-size: 0.95rem;
		&:hover {
			/* text-decoration: underline; */
			color: goldenrod;
		}
	}

	.link-item {
		text-decoration: none;
		color: maroon;
		font-weight: 600;
		font-size: 0.95rem;
		&:hover {
			text-decoration: underline;
			/* color: #1b1b1b; */
		}
	}

	.demo-item {
		text-decoration: none;
		color: goldenrod;
		font-weight: 600;
		font-size: 0.95rem;
		&:hover {
			text-decoration: underline;
			/* color: #1b1b1b; */
		}
	}

	.content {
		padding: 0px 0 0px;
		display: grid;
		gap: 0px;
	}

	.final-project {
		padding: 0px 0 80px;
		max-width: 80%;
		margin: 0 auto;
	}

	.semester-filter {
		display: grid;
		gap: 8px;
		margin: 0 auto 12px;
		max-width: 90%;
		font-weight: 600;
	}

	.semester-buttons {
		display: flex;
		flex-wrap: wrap;
		gap: 8px;
	}

	.semester-buttons button {
		padding: 6px 12px;
		border-radius: 999px;
		border: 1px solid #d0d0d0;
		background: #ffffff;
		font-size: 0.9rem;
		cursor: pointer;
	}

	.semester-buttons button.active {
		background: maroon;
		color: gold;
		/* border-color: ; */
	}

	.project-grid {
		display: grid;
		grid-template-columns: repeat(auto-fit, minmax(350px, 1fr));
		gap: 50px;
		margin-top: 16px;
		row-gap: 40px;
	}

	.project-card {
		border: 1px solid #e5e5e5;
		border-radius: 8px;
		padding: 20px;
		display: grid;
		gap: 8px;
		background: #ffffff;
	}

	.project-teaser {
		width: 100%;
		height: 200px;
		object-fit: cover;
		border-radius: 8px;
		background: #f2f2f2;
		transition: transform 0.15s ease;
	}

	.project-teaser:hover {
		cursor: pointer;
		transform: scale(1.01);
	}

	.project-title {
		font-size: 1.05rem;
		margin: 0;
		display: inline-block;
		transform-origin: left center;
		transition: color 0.1s ease, transform 0.05s ease;
	}

	.project-card:hover .project-title {
		color: #8ace00;
		transform: scale(1.06);
		/* text-decoration: underline; */
	}

	.project-semester,
	.project-members {
		margin: 0;
		color: #4f4f4f;
		font-size: 0.95rem;
	}

	.staff-link {
		color: inherit;
		text-decoration: none;
	}

	.staff-link:hover {
		/* color: gray; */
		text-decoration: none;
		font-weight: 600;
	}

	.staff-title {
		margin: 0 0 0px;
	}

	.staff-grid {
		display: grid;
		grid-template-columns: repeat(auto-fit, minmax(260px, 1fr));
		gap: 12px 20px;
	}

	.staff-card p {
		margin: 0;
		line-height: 1.4;
	}

	.staff-card p + p {
		margin-top: 10px;
	}

	.error {
		color: #b42318;
	}

	.resources {
		max-width: 75%;
		margin: 0 auto 100px;
	}

	.resources-list {
		margin: 0;
		padding-left: 20px;
		display: grid;
		gap: 8px;
	}


</style>
