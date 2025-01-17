# deployment-server

## vercel.json
- {
  "version": 2,
  "builds": [
    {
      "src": "index.js",
      "use": "@vercel/node"
    }
  ],
  "routes": [
    {
      "src": "/(.*)",
      "dest": "index.js",
      "methods": ["GET", "POST", "PUT", "PATCH", "DELETE", "OPTIONS"]
    }
  ]
}

## Production Domain 
- //Must remove "/" from your production URL
app.use(
  cors({
    origin: [
      "http://localhost:5173",
      "https://your website name url.web.app",
      "https://your website name url .com",
    ],
    credentials: true,
  })

## Cookie Options 

- const cookieOptions = {
  httpOnly: true,
  secure: process.env.NODE_ENV === "production",
  sameSite: process.env.NODE_ENV === "production" ? "none" : "strict",
};
//localhost:5000 and localhost:5173 are treated as same site.  so sameSite value must be strict in development server.  in production sameSite will be none
// in development server secure will false .  in production secure will be true

## modify cookie 
- //creating Token
app.post("/jwt", logger, async (req, res) => {
  const user = req.body;
  console.log("user for token", user);
  const token = jwt.sign(user, process.env.ACCESS_TOKEN_SECRET);

  res.cookie("token", token, cookieOptions).send({ success: true });
});

//clearing Token
app.post("/logout", async (req, res) => {
  const user = req.body;
  console.log("logging out", user);
  res
    .clearCookie("token", { ...cookieOptions, maxAge: 0 })
    .send({ success: true });
});

## Deploy Command 

- vercel --prod
-   Set up and deploy -> y
-   Which scope should contain your project? select Account-> Enter
-   Link to existing project? -> n
-   What’s your project’s name? -> Enter
-   In which directory is your code located? -> Enter

## Go to Vercel Your New Project Setting 
- go to settings
- go to Environment Veriabls
- select import file
- select your env file from your Local Storage
- save
