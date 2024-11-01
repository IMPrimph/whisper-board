Whisper Board
	- A website and app where students can lodge anonymous complaints on theis organization, upvote, share them, mark them as resolved based on consensus.

Features
	- User can lodge anonymous complaints againt their institution
	- Users can view existing complaints and upvote them
	- Complaints are tied to particular institution (once institution is entered, its is locked)
	- If institution isnt present on map, they can add manually by entering a few details and it will be popped in maps next time
	- Users can share complaints with social media
	- Users can find all their complaints lodged and its status

Pages
	Sign In Page:
		- With google or (email and password)
		- Forgot Password functionality
	Forgot Password page:
		- Enter email
		- Forgot Password link to be sent to email if email found
		- New password
		- Confirm new password
	Sign Up Page:
		- With google or (email and password)
		- Email verification is required (4 digit code entering should do)
		- Name, instituition selection
	Home Page:
		- Search bar at top (can search by complaint title)
		- Filter option beside that (Latest, Oldest, Top Voted - default)
		- Scroll to bottom should fetch more complaints
		- Every complaint will have
			- Title
			- Short Description
			- View more button
	Complaint View Page - Non autenticated:
		- Title
		- Full Description
		- Complaint category
		- Posted date
		- Upvote button with count (user can upvote a complaint only once)
		- Share button
		- A resolved button (will be present only for the complaints that are 3 or more days old) - User response will also be recorded for this and they wont be asked again for that complaint
	Profile Page:
		- User details, everything is editable except (email, institution)
		- History of the complaints they filed (same as home page, but their specific complaints)
		- Sign Out option
	Share App Page
		- This has to incorporated somewhere, not sure where on UX
		- A custom link with institution stored in url will be generated and can be shared
		- While a new user is onboarding, institution will be prefilled based on the link
	Complaint Page
		- Will contain the following fields
			- Title
			- Description
			- Category
		- User will be acknowledged once complaint is posted successfully

API
	POST /auth/signIn
		req_body: {
			"email": "123@gmail.com",
			"password": "1234",
			"google_token": "euhiurgwieu" -- this is if user is signing by google
		}

		response_body: {
		  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
		  "user_id": 123,
		  "place_id": "place_123"
		}

	POST /auth/forgotPassword
		can pick later

	POST /auth/resetPassword
		can pick later

	POST /auth/signUp
		req_body: {
		  "name": "vis",
		  "email": "vis@gmail.com",
		  "password": "pass1234",
		  "institution_name": "blkbox",
		  "place_id": "place_123", // google will give this
		  "latitide": "22.333",
		  "longitude": "20.22",
		  "region": "hyderabad",
		  "google_token": "qwe3243ere" // incase of google login
		} -- few things might changes here based on google maps integration api

		response_body: {
			"token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
			"user_id": 123,
			"place_id": "place_123",
			"verification_code": 4567 -- to be used once the enter 4 digit code screen comes up
		}

	POST /auth/verify-email
		req_body: {
			"verification_code": 4567,
			"user_id": 123
		}

		response_body: {
		  "message": "Email verified successfully"
		}

	GET /complaints -- No Auth (if user is not logged in, should search institution first, else place_id is being sent in signIn and signUp apis)
		req_body: {
			"place_id": "place_123"
			"user_id": "123" -- optional, based on whethere user logged in or not
		}

		query_params: 
			search: String (complaint title search)
			filter: String (latest, oldest, topVoted)

		response_body: {
		  "complaints": [
			  {
				  "id": 1,
			      "title": "Sample Complaint",
			      "full_description": "This is a sample description",
		      	  "upvote_count": 10,
		      	  "created_at": "2023-03-01T12:00:00.000Z",
		      	  "is_upvoted": true,
		      	  "category": "Academics",
		      	  "is_resolved": false
	      	  }
	      ]
		}

	GET /complaints/:id - Auth Required
		params: id (Complaint ID)
		can pick later (this is primaraily added for comments purpose in V2, from UI this can be a modal that opens once user clicks on a specific complaint)

	POST /complaints/:id/mark-resolved - Auth Required (From Ui side, user who owns the complaint should only be able to hit this api)
		query_params: id (Complaint ID)

		req_body: {
			"status": "not_resolved"
		}

		response_body: {
			"status": "success"
		}

	API: GET /profile - Auth required
		response_body: {
		  "user": {
		    "id": 123,
		    "name": "John Doe",
		    "email": "johndoe@example.com",
		    "institutionId": 456
		  },
		  "complaints": [
		    {
				  "id": 1,
			      "title": "Sample Complaint",
			      "full_description": "This is a sample description",
		      	  "upvote_count": 10,
		      	  "created_at": "2023-03-01T12:00:00.000Z",
		      	  "is_upvoted": true,
		      	  "category": "Academics",
		      	  "is_resolved": false
	      	}
		  ]
		}

	PATCH /profile - Auth required
		may be later

	POST /share-app-link - No Auth
		response_body: {
		  "share_link": "https://example.com/share?institutionId=456"
		}

		-- On mobile this can open social apps popup and choose the destination

	POST /complaints - Auth required
		req_body: {
			"title": "name",
			"description": "bbasjdhjagsd",
			"place_id": "place_123"
		}

		response_body: {
		  "message": "Complaint posted successfully",
		  "complaint_id": 1
		}



V2 features
	- Commenting on complaints
	- Optional media attachment with complaint
	- TFIDF check among the existing complaints in recent days, if this is same as others
	- Add instituition if it isnt found on maps
	- Institution change functionality

To think of
	- Show a way to user to mark a complaint as resolved instead of annoying them
