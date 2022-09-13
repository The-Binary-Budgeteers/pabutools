# pabutools---tools for participatory budgeting

Pabutools is a pythonic package providing the implementation of common PB voting rules as well as the Pabulib parser.
It provides two submodules: **model** and **rules**.
 ## model
 Provides the classes representing voters, candidates and election instances.
  - **Voter(id, sex=None, age=None, subunits=set())** with the following attributes:
	- id: unique ID of the voter,
	- sex: 'M' (male), 'F' (female) or None (no information),
	- age: a natural number or None (no information),
	- subunits: a set of strings (subunits which the voter belongs to).
  - **Candidate(id, cost, name=None, subunit=None)** with the following attributes:
	- id: unique ID of the candidate,
	- cost: a natural number,
	- name: string or None (no information),
	- subunit: string or None (no information or a citywide project).
  - **Election(voters=set(), profile={}, budget=0, country = None, unit = None, subunits = set(), year = None)** with the following attributes:
	- voters: a set of Voter instances,
	- profile: a dict where keys are Candidate instances and values are dicts in which keys are Voter instances and values are natural numbers (voters' utilities over candidates; if a voter has utility 0, she can be skipped),
	- budget: a natural number,
	- country: string or None (no information),
	- unit: string or None (no information),
	- subunits: a set of strings (subunits within the instance),
	- year: int or None (no information).

	and the following method:
	- read_from_file(pattern): takes as an input the pattern of the filepaths of Pabulib files and fills the data of the newly created Election instance with them (e.g. calling Election().read_from_files('pabulib/poland_warszawa_2021*') will create a PB election out of all Pabulib files under the provided path starting with 'poland_warszawa_2021'). If the pattern fits more than one file, they should all be from the same country, unit and year.
 ## rules
Provides the implementation of a number of voting rules for PB. All the methods take as an argument an election instance and return a set of elected candidates.
 - **utilitarian_greedy(e : Election)**: the simple greedy algorithm,
 - **phragmen(e : Election)**: the Phragmen's Sequential Method,
 - **method_of_equal_shares(e : Election, completion='binsearch')**: the method of equal shares. The 'completion' parameter can take the following values:
	 - 'binsearch': by default. Then the initial endowments of the voters are set to maximize the exhaustiveness of the elected committee with the use of binary search.
	 - None: no completion.
	 - 'utilitarian_greedy': the completion with utilitarian_greedy.
	 - 'phragmen': the completion with the Phragmen's Sequential Method.
