# Creates a markdown that combines all the papers section into a md and tex file.
# The resulting tex is NOT standalone since I want to go to and from overleaf. A template input file would be required to generate a semi-clean tex file, which I don't have the patience to do
# Can also do the opposite of converting merged tex (overleaf) file back into md file, which can then be splitted into multiple md files (1x/section).  

# The docs that get version controlled (section01.md, etc and body_combined.tex) are in the same folder as the makefile. The newly modified/created "output" are in the build folders. 

# So if you want to create new section md files for mkdocs from overleaf, you must put body_frontiers.tex from OL in makefile folder. The new section01.md, section02.md will appear in the buildmd. 

# If you want to generate a new body_frontiers.tex for overleaf from mkdocs section01.md, then run make tex, and transfer the new version from buildtex to overleaf.


#========== ATOMIX WORKFLOW ==============#
# Generally speaking, we'll be modifying the overleaf document and regenerating md files for web commenting/reviewing. Which involves the following workflow
# 1. Work/edit overleaf
# 2. Copy body_frontiers.tex into the (mkdocs) makefile folder
# 3. Run make clean in same folder
# 4. Run make md_from_tex while will create a merged.md document 
# 5. Run make split_md to generate  section01.md, section02.md, etc.
# 5. Move section01.md, etc from buildmd into the main (makefile) folder, after inspecting them manually ( I can change the code when I'm more confident). These will now be the files that get used to build documents
# 6. mkdocs build the whole website, after ensuring all section01.md are included in mkdocs.yml file
# 7. Push mkdocs git to guthub.  
 
 
# requires mdsaw executable (python) and pandoc 

BUILDDIR=buildtex
BUILDMD=buildmd
FILENAME=body_frontiers
MDFILENAME=merged


clean:
	echo "Remove all build files"
	if [ -d $(BUILDDIR) ]; then rm -rf $(BUILDDIR); fi
	if [ -d $(BUILDMD) ]; then rm -rf $(BUILDMD); fi

  

#citeproc
tex: 
	mkdir -p $(BUILDDIR)   # Creates the BUILDDIR if it doesn't already exist.
	pandoc  *.md  -o  $(BUILDDIR)/$(FILENAME).md 
	pandoc $(BUILDDIR)/$(FILENAME).md  \
	--natbib  \
    --bibliography="myCEBlib.bib" \
    --csl="frontiers.csl" \
    --from=markdown+tex_math_single_backslash+tex_math_dollars+raw_tex-auto_identifiers \
    --to=latex \
    --output=$(BUILDDIR)/$(FILENAME).tex \
    
    #citeproc. Generates 1x md file
md_from_tex: 
	mkdir -p $(BUILDMD)   # Creates the BUILDDIR if it doesn't already exist.
	pandoc $(FILENAME).tex \
	--natbib  \
    --bibliography="myCEBlib.bib" \
    --csl="frontiers.csl" \
    --to=markdown+tex_math_single_backslash+tex_math_dollars+raw_tex-auto_identifiers \
    --from=latex \
    --output=$(BUILDMD)/$(MDFILENAME).md 
	
    
split_md:
	./mdsaw -d $(BUILDMD)/$(MDFILENAME).md $(BUILDMD)

	