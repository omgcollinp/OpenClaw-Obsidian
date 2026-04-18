# Instructions
1. Check the YouTube playlist - https://www.youtube.com/playlist?list=PLmB5vCF1F5eiRPw7-gQvmoerp0n3_OHQj
2. Select the next video
   1. Before processing, check the processed log in the vault to avoid duplicates:
      - `/Users/collin/Documents/ParteeLabs/09_Scripts/Youtube Video Analyzer/processed-log.md`
   2. Use workflow helper to get next pending video:
      - `python3 "/Users/collin/Documents/ParteeLabs/09_Scripts/Youtube Video Analyzer/scripts/youtube_video_workflow.py" next`
   3. After analysis note is created, mark video as processed:
      - `python3 "/Users/collin/Documents/ParteeLabs/09_Scripts/Youtube Video Analyzer/scripts/youtube_video_workflow.py" mark <video_id> "<obsidian-note-path>.md"`
3. Use the Python script `"/Users/collin/Documents/ParteeLabs/09_Scripts/Youtube Video Analyzer/scripts/extract_mp3.py"` to download the MP3 of the video
	1. Example: `python3 "/Users/collin/Documents/ParteeLabs/09_Scripts/Youtube Video Analyzer/scripts/extract_mp3.py" "<youtube-url>" -o "/Users/collin/Documents/ParteeLabs/09_Scripts/Youtube Video Analyzer/downloads"`
4. Use AssemblyAI to transcribe the video with `"/Users/collin/Documents/ParteeLabs/09_Scripts/Youtube Video Analyzer/scripts/transcribe_assemblyai.py"`
	1. Optional: activate venv first: `source "/Users/collin/Documents/ParteeLabs/09_Scripts/Youtube Video Analyzer/scripts/.venv_yta/bin/activate"`
	2. see docs [here](https://www.assemblyai.com/docs/getting-started/transcribe-an-audio-file)
	3. set env var first: `export ASSEMBLYAI_API_KEY="23d0686c0eee4faab9c3e88f08b6d65f"`
	4. run: `python3 "/Users/collin/Documents/ParteeLabs/09_Scripts/Youtube Video Analyzer/scripts/transcribe_assemblyai.py" --file "/Users/collin/Documents/ParteeLabs/09_Scripts/Youtube Video Analyzer/downloads/<file>.mp3" --out "/Users/collin/Documents/ParteeLabs/09_Scripts/Youtube Video Analyzer/transcripts/<file>_assemblyai" --language-detection --speaker-labels`
5. Use Codex 5.3 to analyze the video use one of these prompts:
	1. EYL Market Monday Video: [[09_Scripts/Youtube Video Analyzer/video-analysis.prompts/Market Mondays.prompt]]
	2. Jack Mallers Video: [[01_Projects/tech/Video Analyzers/video-analysis.prompts 1/Jack Mallers Show.prompt|Jack Mallers Show.prompt]]
	3. All Other Videos: [[01_Projects/tech/Video Analyzers/video-analysis.prompts 1/YouTube Analyzer.prompt|YouTube Analyzer.prompt]]
6. Get the analysis and create a new note in the Notes directory.
	1. use front matter for some stuff like name, date, and tags.
7. Add a link to the newly created note in the Daily Note under Links.